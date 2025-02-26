# OWASP Top 10 Overview

The OWASP Top 10 is a list of the most critical security risks to web applications, maintained by the Open Web Application Security Project (OWASP). It serves as a guideline for developers, security professionals, and organizations to identify and mitigate common security vulnerabilities.
OWASP Top 10 Security Risks

Each entry in the OWASP Top 10 represents a significant security risk that can lead to data breaches, system compromises, and other malicious activities.

#### Note: Currently OWASP are surveying to update this in 2025, updates to these notes will need to be studied and made at some point this year (Make this page and existing folder legacy 2021 and a new 2025 version)

## Top 10 List

1. Broken Access Control

    Improperly enforced access restrictions can allow unauthorized users to access, modify, or delete sensitive data.

    Mitigation: Implement proper role-based access control (RBAC), enforce least privilege, and validate authorization at every level.

2. Cryptographic Failures

    Inadequate protection of sensitive data due to weak or misconfigured cryptographic mechanisms.

    Mitigation: Use strong encryption standards (AES-256, RSA-2048), secure key management, and avoid outdated algorithms.

3. Injection

    Occurs when untrusted data is sent to an interpreter (e.g., SQL, NoSQL, OS commands) and executed as part of a query or command.

    Mitigation: Use parameterized queries, prepared statements, and input validation to prevent code execution.

4. Insecure Design

    Flaws in architecture and design lead to vulnerabilities that cannot be easily patched.

    Mitigation: Implement threat modeling, secure design patterns, and rigorous code reviews from the early stages of development.

5. Security Misconfiguration

    Default settings, unnecessary features, or incomplete configurations that expose vulnerabilities.

    Mitigation: Harden configurations, remove unused components, and use automated security scanning tools.

6. Vulnerable and Outdated Components

    Using outdated libraries, frameworks, or software with known vulnerabilities.

    Mitigation: Regularly update dependencies, monitor vulnerability databases, and apply security patches.

7. Identification and Authentication Failures

    Weak authentication mechanisms leading to account takeovers, session hijacking, and unauthorized access.

    Mitigation: Implement multi-factor authentication (MFA), enforce strong password policies, and secure session management.

8. Software and Data Integrity Failures

    Compromised updates, insecure CI/CD pipelines, or unverified dependencies leading to unauthorized modifications.

    Mitigation: Use code signing, integrity checks, and secure update mechanisms.

9. Security Logging and Monitoring Failures

    Lack of proper logging and monitoring can delay detection and response to security incidents.

    Mitigation: Implement centralized logging, real-time monitoring, and automated alerting systems.

10. Server-Side Request Forgery (SSRF)

    Attackers manipulate requests sent from a server, potentially accessing internal resources.

    Mitigation: Restrict outbound requests, validate input, and implement allow-lists for network access.

By addressing these security risks, developers can build more secure, resilient, and robust applications that protect user data and organizational assets. Awareness and proactive mitigation of these vulnerabilities are essential for modern software development.


