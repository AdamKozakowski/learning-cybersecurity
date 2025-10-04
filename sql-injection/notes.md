-- SQL Injection – understanding injection techniques and prevention methods.
# SQL Injection — Notes

## Definition
SQL Injection (SQLi) is a class of injection attacks that occurs when untrusted user input is incorporated into a SQL query in an unsafe way. As a result, an attacker can manipulate the query logic so that the database performs actions the developer did not intend. Because relational databases are so widespread, SQLi is among the most common web vulnerabilities.

In short: if your application is vulnerable to SQL Injection, an attacker may execute arbitrary SQL commands against your database.


## Impact and risks
What can go wrong if an application is vulnerable to SQL Injection?

- **Unauthorized data access** — attackers can read sensitive information (personal data, payment details, credentials).  
- **Data exfiltration** — harvesting data silently and using it elsewhere (fraud, credential stuffing).  
- **Data modification or destruction** — attackers can update, delete, or drop tables, corrupting the database and breaking the application.  
- **Privilege escalation and command execution** — complex attacks can enable the attacker to escalate privileges or run additional malicious code.  
- **Reputation and legal consequences** — breaches can harm users, damage trust, and cause regulatory penalties.

Real-world incidents show that SQLi has been used to compromise high‑profile services, leak credentials, and extract sensitive records. Even security-conscious organisations have suffered when SQLi was present.

## Example (educational only)


## Prevention and mitigation
Key defensive measures to prevent SQL Injection:

### Use parameterized queries / prepared statements
Always bind user-supplied values as parameters instead of concatenating them into SQL strings. This separates code from data and prevents injected payloads from being interpreted as SQL.

 ### Validate and sanitize input
Enforce strict validation rules (type, length, format). Reject or normalize unexpected values early (for example: validate emails with a regex and limit length).

### Principle of least privilege
Run the application with a database account that has only the minimum required permissions (e.g., EXECUTE on stored procedures, SELECT on specific views). Avoid granting broad rights like db_owner.

### Prefer stored procedures or parameterized modules
Encapsulate database access in stored procedures or views that accept parameters. Grant the application permissions to execute those procedures rather than direct table access.

### If using dynamic SQL, use safe APIs
When dynamic SQL is unavoidable, use parameterized execution (e.g. sp_executesql on SQL Server) and escape identifiers with functions like QUOTENAME().

### Apply defense‑in‑depth
Combine multiple controls: WAF, input validation, least privilege, logging/monitoring, and code reviews.

### Auditing and monitoring
Log database activity and enable alerts for suspicious queries, unusual volumes of data access, or pattern matching indicative of injection attempts.

### Testing
Regularly test in safe environments (OWASP Juice Shop, DVWA, dedicated labs). Include automated security scanning and periodic manual penetration tests.
Programming languages talk to SQL databases using database drivers. A driver allows an application to construct and run SQL statements against a database, extracting and manipulating data as needed. Parameterized statements make sure that the parameters (i.e. inputs) passed into SQL statements are treated in a safe manner.

## Sources
Hacksplaining SQL Injection
