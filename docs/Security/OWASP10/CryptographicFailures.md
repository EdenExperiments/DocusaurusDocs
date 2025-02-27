# Cryptographic Failures

A Cryptographic failure referes to any vulnerability arisiign from the misuse (or the lack of use) of cryptographic algorithms for protecting sensitive information. Web apps require cryptography to provide confidentiality for their users at many levels.

For an example, email clients should encrypt the data of the email before sending to the server, so that it cannot be read via an eavesdropper and get access to email addresses and content of the email.

Additionally they may be encrypted on the server too, and only decrypted via keys when sent to clients. 

Generally a cryptographic failure ends up divulging sensitive data, often linked to customers scuh as personal data and passwords.

## Bad Generic Example 

Suppose a client encrypts a username and password before sending to a server, an eavesdropper could take the encrypted value and attempt to login as a user if they are able to crack it. If no encryption is used at all, they dont even need to try!

So suppose an eavedropper was able to obtain hash 6eea9b7ef19179a06954edd0f6c05ceb, if using a weak hash, they could use a tool such as crackstation.net, or more expansive tools and see what the password is (especially if users are using weak passwords) like below.

![Crackstation site breaking hash to a commmon password](/img/crackstation.png)

## Bad Code C# Example


## Example Script to exploit




## How to mitigate Examples
