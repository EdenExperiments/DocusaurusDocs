# Broken Access Control

Broken Access Control or Broken Object Level Authorisation (BOLA) occurs when an API fails to enforce proper permissions for objects within its endpoints. BOLA allows attackers to manipulate object identifiers (such as IDs) to access and modify resources which they should not have access to. This is typically down to inadequate authorisation checks within an API.

An example of this may be allowing data to be retrieved by modifying parameters within a request or directly in a URL such as orderID, or attempting urls which you shouldn't have access too, but without sufficient checks HTML is supplied regardless.

In 2024 it was reported over 75% of API vulnerabilities were due to inadequate access control. 

## Bad Code C# Example

In an example API, an endpoint for getting reservations for a user only expects a non-encrypted id of the user and no checks are completed, allowing someone to obtain info they shouldn't be changing this ID within the request. 

```
public IActionResult GetReservations(string id) {

    //Fetch reservations for the current user
    var reservations = dbContext.Reservations
        .Where(r => r.UserId = id)
        .ToList();

    return OK(reservations);
}
```

At its worst, someone could brute force this API from 1 until it gets no results and obtain data on all users, resulting in a mass-scale data breach.

It is important to note an endpoint like this might be a POST or DELETE or anything to, accepting just an ID, or an ID and a string/json object, which would result in mass modification or deletion of data. Even with backups, an e-commerce site could lose an entire day's worth of orders or new accounts. Worse, it could damage customer trust and bring on fines.

This is a very simple attack to discover and exploit, so it is important to minimise these.

## Example Script to exploit

```
#!/bin/bash

# File to save the output
output_file="reservations.txt"

# Clear the output file before starting
> "$output_file"

# Loop through IDs 1 to 100
for id in {1..100}; do
  # Fetch the URL and append the output to the file
  url="https://example.com/users/${id}/reservations"
  echo "Fetching data for ID: $id from $url"
  
  # Use curl to access the URL and append the response to the file
  curl -s "$url" >> "$output_file"
  
  # Separate each user's response with a delimiter for readability
  echo -e "\n--- End of ID $id ---\n" >> "$output_file"
done

echo "All data has been saved to $output_file"
```

## How to mitigate Examples

Mitigating these exploits depends on the nature of the application, but in general it is the useage of policies and server-side checks to validate user permissions for each resource. It is best to be able to do this as close to the front-end as possible, e.g. Middleware, framework features to enforce checks globally.

Another is to not use any predictable ID within the endpoint parameters such as a users UserID, but to use a unique identifier which is a 128-bit number or 36 character string (For Example using NEWID() within a SQL Server backend to create a unique identifier). 

Below goes into further explanation of some additional mitigations

### Checking ids against context set during logins

In the below example, we retrieve the authenticated user ID from the HTTP context, and compare that to the id parameter provided in the request. If these do not match, then we can respond with a Forbidden Status and a message indicating unauthorized access.

Only after these checks do we complete the getting/modification/deletion of any data.

```
public IActionResult GetReservations(string id) {
    string currentUserId = HttpContext.User.Identity.Name; // Assume user ID is retrieved from the authenticated context

    // Ensure the logged-in user is authorized to access this data
    if (currentUserId != id) {
        return Forbid("Unauthorized access");
    }

    // Fetch reservations for the current user
    var reservations = dbContext.Reservations
        .Where(r => r.UserId == currentUserId)
        .ToList();

    return Ok(reservations);
}
```

## Role-Based Access Control (RBAC)

