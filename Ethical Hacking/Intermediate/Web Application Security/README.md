## Web Application Security

Web application security focuses on protecting websites and web applications from common threats, attacks, and vulnerabilities. Ethical hackers use various tools and techniques to identify and exploit these vulnerabilities, helping organizations improve their security posture before attackers can exploit weaknesses.

### OWASP Top 10 Vulnerabilities

| **Vulnerability**                              | **Description**                                                                                                 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| **[SQL Injection](https://github.com/brad-it/BradIT/blob/main/Ethical%20Hacking/Intermediate/Web%20Application%20Security/Attacks/sql-injection.md)** | Occurs when untrusted data is sent to an interpreter (e.g., SQL, OS commands), allowing attackers to manipulate databases or systems. |
| **Broken Authentication**                      | Problems with authentication and session management, allowing attackers to impersonate users or bypass authentication mechanisms. |
| **Sensitive Data Exposure**                    | Sensitive data (e.g., passwords, credit card information) is not properly protected (e.g., through encryption), leading to potential theft. |
| **XML External Entities (XXE)**                | Exploiting XML parsers to access sensitive data or internal files by sending malicious XML input.              |
| **Broken Access Control**                      | Failing to enforce proper restrictions on what authenticated users can do, enabling unauthorized access to data or functionality. |
| **Security Misconfiguration**                  | Incorrect configuration of security settings (e.g., open ports, weak passwords, unnecessary services) that could expose vulnerabilities. |
| **Cross-Site Scripting (XSS)**                  | Malicious scripts are injected into web pages viewed by other users, allowing attackers to steal session cookies or perform unauthorized actions. |
| **Insecure Deserialization**                   | Deserializing untrusted data that can lead to remote code execution or other vulnerabilities if processed incorrectly. |
| **Using Components with Known Vulnerabilities** | Using outdated or insecure libraries or components with known flaws, which could be exploited by attackers to gain access. |
| **Insufficient Logging & Monitoring**          | Failing to properly log activities or monitor systems, making it harder to detect and respond to security breaches. |

### Tools Used by Ethical Hackers

Ethical hackers use a variety of tools to test the security of web applications and identify vulnerabilities. Some common tools include:

- **Burp Suite**: A comprehensive web application security testing tool used for finding vulnerabilities like XSS, SQL Injection, and more.
- **OWASP ZAP (Zed Attack Proxy)**: An open-source tool that helps detect vulnerabilities such as SQLi and XSS, offering both automated scanners and manual testing tools.
- **Nikto**: A web server scanner that identifies potential vulnerabilities, such as outdated software and configuration weaknesses.
- **Nmap**: A network scanning tool to discover open ports and services, helping ethical hackers identify attack vectors.
