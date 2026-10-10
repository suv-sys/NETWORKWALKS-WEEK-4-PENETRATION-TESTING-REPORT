# NETWORKWALKS-WEEK-4-PENETRATION-TESTING-REPORT

NETWORKWALKS WEEK 4 — PENETRATION TESTING REPORT
Target Information
- Target: Mediroza General Hospital
- Target URL: https://medirozahospital.com/
- Assessment Type: Authorized black-box web application security assessment
- Purpose: Educational cybersecurity assessment conducted as part of the NetworkWalks training program

  
Milestone 1 — Initial Reconnaissance
The robots.txt file was reviewed and contained the following entries:
- /patient/
- /staff/
- /old/
The login page at patient/login.php returned different responses for a nonexistent username and an incorrect password. A single quote in the username input also caused a MySQL syntax error to be displayed.
These observations support the reported findings of username enumeration and SQL injection in the login flow.



Milestone 2 — Patient PDF Review
The lab workflow covered password-protected patient-report PDF files. The assessment notes recorded that the PDF passwords were recoverable using the lab's password-recovery workflow.
The findings summary reports that PDFs were accessible after the login bypass and that weak PDF passwords could be recovered with a wordlist. Patient-report contents and passwords are intentionally not reproduced in this repository.



Milestone 3 — Deep Reconnaissance
The path /old/ displayed a directory index containing mediroza_db_backup_2019.sql.
The SQL backup contained staff records, including job titles, departments and salaries, and shareholder records, including ownership percentages and share classes. The PDF metadata also contained an author identifier and a comment referring to the /old/ backup location. The identifier correlated with a staff record for an IT Systems Administrator; this correlation does not establish intent or responsibility.



Milestone 4 — Findings Summary
#	Vulnerability	Location	Risk
1	Username enumeration on login page	patient/login.php	Medium
2	SQL injection login bypass	patient/login.php	Critical
3	Encrypted PDFs accessible after login bypass	patient/reports/	High
4	Weak PDF passwords recoverable with a wordlist	patient_report_*.pdf	High
5	Sensitive metadata in patient PDF files	patient_report_3.pdf	Medium
6	Forgotten backup folder with directory listing enabled	/old/	Critical
7	Confidential staff salaries and shareholder data in plaintext	/old/mediroza_db_backup_2019.sql	Critical


Recommendations and Remediation
Critical
- SQL injection: Use parameterized queries or prepared statements. Never build SQL queries using raw user input.
- Directory listing and backup exposure: Disable directory listing on all folders and remove or relocate old backup files. Never store database backups in a public web folder.
- Confidential backup data: Restrict backup access, review relevant access logs, and assess any applicable incident-response and notification requirements.
High
- PDF access control: Store PDFs outside the web root or serve them through proper server-side access controls. Do not rely on PDF passwords as the only access-control mechanism.
- Weak PDF passwords: Use strong, unique passwords when PDF encryption is required and ensure that access to reports is controlled by the application.
Medium
- Username enumeration: Show the same error message for an incorrect username and an incorrect password.
- PDF metadata: Remove unnecessary metadata from patient files before distribution and verify the cleaned files before release.
Conclusion
The assessment documented weaknesses in authentication behavior, SQL query handling, patient-report access control, PDF protection and metadata handling, and backup-file exposure. The Critical findings should be addressed first, followed by the High- and Medium-risk issues. Remediation should be validated through a controlled retest within the authorized scope.
Disclaimer
This document is produced by NetworkWalks for educational purposes only. The target is described by the lab as authorized for security testing. These techniques must never be applied to any system without explicit written permission from its owner.
Evidence handling: Do not commit raw database backups, unredacted patient PDFs, recovered passwords, national ID numbers, phone numbers, or other confidential personal data to this repository. Add only evidence approved by the instructor and redacted as necessary.





