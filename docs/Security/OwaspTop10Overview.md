# OWASP TOP 10 (2021)

Add Brief rundown

## 1. Broken Access Control

Broken Access Control or Broken Object Level Authorisation (BOLA) occurs when an API fails to enforce proper permissions for objects within its endpoints. BOLA allows attackers to manipulate object identifiers (such as IDs) to access and modify resources which they should not have access to. This is typically down to inadequate authorisation checks within an API.

An example of this may be allowing data to be retrieved by modifying parameters within a request or directly in a URL such as orderID, or attempting urls which you shouldn't have access too, but without sufficient checks HTML is supplied regardless.

In 2024 it was reported over 75% of API vulnerabilities were due to inadequate access control. 

