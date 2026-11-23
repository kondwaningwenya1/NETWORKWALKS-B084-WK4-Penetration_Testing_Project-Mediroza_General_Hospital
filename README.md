<img width="1366" height="768" alt="Whatweb and nslookup" src="https://github.com/user-attachments/assets/f2b8befd-ef04-43c5-a5b0-ec53fcd8cfed" /># NETWORKWALKS-B084-WK4-PENETRATION-TESTING-PROJECT
# PENETRATION TESTING REPORT
## MEDIROZA GENERAL HOSPITAL – FULL PENETRATION TESTING PROJECT
### W4-PM-FINAL | CYBERSECURITY | NETWORKWALKS
### Project Information	Details
| Pentester Name |	Kondwani Ngwenya |
| :--- | :---|
| Program/Batch	| Networkwalks – B084 |
| Date	| 23 November 2026 |
| Project	Week 4 |  Penetration Testing Project |
| Client/Target |	Mediroza General Hospital |
| Target Domain	| https://medirozahospital.com |
| Testing Type |	Black-box Penetration Testing |
| Permission	| Educational testing environment, as authorized by the internship |
| Phases Covered |	M1: Initial Access; M2: Data Extraction;<br> M3: Further Data Exposure; M4: Reporting |
| Status |	Completed |
---
## 1. Liability Disclaimer
This penetration-testing project was conducted as part of the Networkwalks Cybersecurity Internship in an educational and authorized testing environment.
The activities described in this report were performed within the assigned project scope for learning and research purposes. The findings and recommendations are intended to demonstrate practical cybersecurity skills and improve understanding of common web application security weaknesses.

This report is intended for educational and security assessment purposes only. The information contained herein should not be used to access, modify or disclose information belonging to systems without appropriate authorization.
The author, instructor and Networkwalks are not responsible for unauthorized use or misuse of the techniques described in this report.

## 2. Introduction
This report presents the findings from the Week 4 Penetration Testing Project conducted as part of the Networkwalks Cybersecurity Internship, Batch B084.
The project involved conducting a black-box penetration test against a simulated hospital environment, Mediroza General Hospital, using the assigned target domain.
The main objective was to identify security weaknesses in the web application, investigate authentication mechanisms, examine protected PDF documents and identify exposed information that could affect the confidentiality of hospital records.<br>
The assessment was divided into four milestones:<br>
•	M1 – Initial Access: Investigating the website's authentication mechanisms and identifying weaknesses that could allow unauthorized access to the patient portal.<br>
•	M2 – Data Extraction: Examining the recovered PDF reports and investigating their password protection.<br>
•	M3 – Further Data Exposure: Investigating an exposed database backup containing employee salary and shareholder information.<br>
•	M4 – Penetration Testing Report: Documenting the findings, their potential impact, risk ratings and recommended security improvements.
The assessment involved Kali Linux, command-line utilities, PDF analysis tools and MySQL. Evidence was collected throughout the project to support the findings presented in this report.

## 3. Tools Used
The following tools and technologies were used during the assessment.
| Tool	| Purpose |
| :--- | :--- |
| Kali Linux	| Operating system used for penetration-testing activities. |
| WHOIS	| Obtaining publicly available domain registration information. | 
| WhatWeb	 | Identifying web technologies and server information. |
| NSLookup | Used to retrieve DNS information about the target domain. |
| Wafw00f | Used to identify whether the target website was protected by a Web Application Firewall (WAF).|
| Nmap |	Discovering open ports and identifying running services.   |
| Curl	| Inspecting HTTP responses, headers and website resources. | 
| Browser(Firefox and Chrome)	| Accessing the target website and examining its web pages. | 
| ExifTool	| Examining PDF document metadata. | 
| pdfinfo |	Inspecting PDF properties and encryption information. |
| John the Ripper |	Recovering PDF passwords during the assigned data extraction exercise. |
| MySQL / MySQL Workbench	| Importing and examining the recovered SQL database backup. |
| Claude | AI-assisted technical guidance, troubleshooting, interpretation of findings,<br> and report documentation during the penetration testing exercise.|

## 4. Activities Performed
### 4.1 M1 – Initial Access
The first milestone involved examining the target website and identifying exposed entry points.
I began by conducting reconnaissance to understand the website's structure and identify the available authentication pages.
The reconnaissance activities identified the patient login and staff login pages:<br>
•	/patient/login.php<br>
•	/staff/login.php<br>
I examined the login pages and their HTML form structures. The patient portal required a username and password, while the staff portal required a staff identifier and password.
The login forms submitted information using the HTTP POST method.
During the authentication testing, I identified an SQL injection-related weakness in the patient login mechanism. The application exposed a database error through the PHP mysqli_query() function.<br>

The error message referenced the MySQL server's SQL syntax, indicating that the application was interacting with a MySQL database.
This finding demonstrated inadequate handling of user input and the exposure of internal database error information.
The initial access milestone was completed, and the evidence collected was used to support the subsequent project activities.

### 4.2 M2 – Data Extraction and PDF Analysis
The second milestone involved examining the PDF laboratory reports recovered during the initial access exercise.
Three PDF files were recovered:<br>
- patient_report_1.pdf
- patient_report_2.pdf
- patient_report_3.pdf
  
I used John the Ripper during the password recovery exercise to investigate the password protection applied to the documents.
After recovering the passwords, I examined the PDF properties using ExifTool and pdfinfo.
The examination revealed that the documents used Standard V2.3 PDF encryption with RC4 and 128-bit encryption.
Additional document metadata was identified, including the document creator and producer.
One notable observation was that the third PDF contained a different author value and indicated the presence of custom metadata.
The exercise demonstrated that password-protected documents can still present security risks when weak passwords are used.

### 4.3 M3 – Further Data Exposure
The third milestone focused on identifying further information exposure within the simulated hospital environment.
During this phase, I discovered a publicly accessible SQL database backup through a misconfigured directory listing.
The backup was available in plain-text SQL format and contained database definitions and insertion statements.
I examined the backup and identified two tables directly related to the project objectives:
#### A. Staff table
The staff table contained the following fields:
- id
- full_name
- job_title
- department
- email
- phone
- national_id
- monthly_salary_zar
- date_joined
  
The monthly_salary_zar field contained the employee salary information required for the milestone.
#### B. Shareholders table
The shareholders table contained:
- id
-	shareholder_name
-	share_percent
- shares_held
- share_class
  
These fields represented shareholder identities and ownership information.
I imported the recovered SQL backup into a local MySQL database on Windows to examine its structure and records.
This confirmed that the exposed backup contained the database tables required to fulfil the M3 objectives.
The finding demonstrated the risk created when database backups containing confidential organizational information are made publicly accessible.

## 5. Risk Analysis / Impact
Based on the activities performed, the following security findings were identified.

No.	Risk / Finding	Evidence / Observation	Potential Impact	Risk Level
1.	SQL injection in patient authentication
SQL-related error exposed through mysqli_query() during authentication testing.	Could allow manipulation of database queries and compromise authentication or database confidentiality.	High
2.	Weak PDF password protection	
The three recovered PDF documents were password-protected using RC4-based PDF encryption.	Weak passwords may allow unauthorized recovery of protected document contents.	High
3.	Misconfigured directory listing
A publicly accessible directory exposed a database backup.	Visitors could discover and access files that should not be publicly available.	High
4.	Exposed database backup
The SQL backup contained staff and shareholder tables with confidential organizational information.	Exposure of employee salary information and shareholder ownership records could cause privacy, financial and reputational harm.	Critical
5.	Information disclosure through application errors
PHP and MySQL error information was exposed during login testing.	Attackers may obtain technical information useful for further attacks.	Medium
- Risk level key:
- Critical – Very serious security impact;
  High – Significant security weakness;
  Medium – Moderate security concern;
  Low – Limited security impact.
  
The risk ratings are based on the observed weaknesses and their potential impact within the simulated environment. The exposed database backup was rated Critical because it contained multiple categories of confidential organizational information.

## 6. Recommendations
Based on the findings from the penetration-testing exercise, the following security improvements are recommended.
1. Prevent SQL injection
The application should use prepared statements and parameterized queries when interacting with the MySQL database. User input should never be directly incorporated into SQL statements.
2. Improve authentication security
The patient and staff login mechanisms should implement secure authentication practices, proper input validation, rate limiting and secure password storage.
3. Strengthen PDF password protection
Sensitive PDF documents should use modern encryption standards and strong, unique passwords. Passwords should not be reused or stored insecurely.
4. Disable public directory listing
Directory browsing should be disabled on the web server unless explicitly required. Sensitive directories should not be accessible to unauthenticated visitors.
5. Protect database backups
Database backups should be stored outside publicly accessible web directories. Access should be restricted to authorized personnel, and backups should be encrypted.
6. Protect employee and shareholder information
Sensitive employee and shareholder records should only be accessible to authorized users with appropriate privileges.
7. Improve application error handling
Detailed PHP and MySQL error messages should not be displayed to ordinary website visitors. Errors should be recorded securely in server-side logs.
8. Conduct regular security assessments
The organization should periodically assess its web applications, authentication mechanisms, server configuration and backup storage arrangements.
9. Implement access monitoring
The organization should monitor access to sensitive files and database backups and investigate suspicious access attempts.

## 7. Conclusion
During Week 4 of my Networkwalks Cybersecurity Internship, I completed a practical penetration-testing project against the simulated Mediroza General Hospital environment.
The project covered the major stages of a penetration-testing engagement, from reconnaissance and initial access to data extraction, further data exposure and professional reporting.

During M1, I examined the hospital website's authentication mechanisms and identified an SQL injection-related weakness in the patient login process.

During M2, I recovered three password-protected PDF laboratory reports and examined their encryption and metadata.

During M3, I discovered a publicly accessible SQL database backup through a misconfigured directory listing. The backup contained staff and shareholder tables with information relevant to the assignment objectives. I imported the backup into a local MySQL environment for examination.

Finally, I documented the findings, assessed their potential impact and developed recommendations to address the identified weaknesses.
This project improved my understanding of how individual vulnerabilities can combine to create serious security risks. It also demonstrated the importance of following a structured testing process, collecting evidence and communicating findings clearly.
The experience reinforced the importance of ethical conduct, proper authorization and responsible handling of sensitive information during cybersecurity assessments.

## 8. Evidences Collected

### M1 – Initial Access
Target website and discovered entry points.
<img width="1366" height="768" alt="Whatweb and nslookup" src="https://github.com/user-attachments/assets/a4f9ea5d-2517-4c95-aba2-9c60be22f8b7" />
<img width="1366" height="768" alt="wafwoof and curl -I" src="https://github.com/user-attachments/assets/1d3601f0-edc5-470d-992d-10aacea31bd8" />
<img width="1366" height="768" alt="nmap scan sv" src="https://github.com/user-attachments/assets/7253c72c-6f8b-4cf6-8e16-61393f014d52" />

Patient login form.

<img width="1366" height="768" alt="patient potal" src="https://github.com/user-attachments/assets/7f7671a6-c1ad-45ff-b727-d2f41e5e891a" />

SQL injection-related error.
<img width="1366" height="768" alt="Patient portal" src="https://github.com/user-attachments/assets/9827e79b-a48b-47d3-a89f-416e204de92b" />


#### M2 – Data Extraction
Three recovered PDF files.
<img width="1366" height="768" alt="patient_report_1_unlocked pdf" src="https://github.com/user-attachments/assets/1da8a805-5521-4fdb-a74f-33545ab5f56a" />
<img width="1366" height="768" alt="patient_report_2_Unlocked pdf" src="https://github.com/user-attachments/assets/d5f1aab8-e238-4bc7-a355-cfd5f8b5782f" />
<img width="1366" height="768" alt="patient_report_3_unlocked pdf" src="https://github.com/user-attachments/assets/98963eed-55ec-4b85-984c-764139a581c0" />



PDF password recovery evidence.
<img width="1366" height="768" alt="John the reaper cracking psswd 2" src="https://github.com/user-attachments/assets/ad714b3c-e00a-481e-9bf6-d3753a97fc79" />

PDF metadata examination.
<img width="1366" height="768" alt="exiftool results" src="https://github.com/user-attachments/assets/84e47239-dcb9-4760-b0a3-88fd21a7d233" />


### M3 – Further Data Exposure
Exposed directory listing.
<img width="1366" height="768" alt="directory listing" src="https://github.com/user-attachments/assets/d8fc5365-0a07-414e-87c4-7fb300fc1aea" />


Accessible SQL database backup.
<img width="1366" height="768" alt="backup database" src="https://github.com/user-attachments/assets/6d83c74f-dec0-43c9-972b-22c13c158b3b" />


employee salary table evidence.
<img width="1366" height="768" alt="SQL querry salary results" src="https://github.com/user-attachments/assets/c4595e26-ef34-4d91-86e7-6d5a5a992ca8" />

shareholder table evidence.
<img width="1366" height="768" alt="SQL query stakeholders details" src="https://github.com/user-attachments/assets/939e89ef-20fa-4eab-97a7-d4b330422c81" />

---
END OF REPORT
| Author:| Kondwani Ngwenya |
| :--- | :--- |
| Program: |  Cybersecurity Program – Networkwalks |
| Batch: B083 |
| Project: | Week 4 – Penetration Testing Project | 
| Instructor: | Waqas Karim (CCIE) | 
| Organization: | Networkwalks | 

