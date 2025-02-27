# Injection

Injection vulnerabilities occur because application interprets user-controlled inputs as commands, whether via system commands or SQL queries as examples. 

The main defense to this is using an allow list when an input is sent to a server, and the input will be compared to safe inputs and approved, or otherwise rejected and log an error.

Additionally, an input may be stripped to remove any dangerous characters.

## Command Injection Examples

