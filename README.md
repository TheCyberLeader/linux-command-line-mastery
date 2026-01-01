# Linux Command Line Mastery 🐧

[![Status](https://img.shields.io/badge/Status-Complete-success)](https://img.shields.io/badge/Status-Complete-success)
[![Projects](https://img.shields.io/badge/Projects-2-blue)](https://img.shields.io/badge/Projects-2-blue)
[![Skills](https://img.shields.io/badge/Skills-Linux%20%7C%20File%20Permissions%20%7C%20SQL-informational)](https://img.shields.io/badge/Skills-Linux%20%7C%20File%20Permissions%20%7C%20SQL-informational)

## Table of Contents

* [Overview](#overview)
* [Portfolio Quick Reference](#-portfolio-quick-reference)
* [Skills Demonstrated](#skills-demonstrated)
* [Project 1: Linux File Permissions Management](#-project-1-linux-file-permissions-management)
  + [Check File and Directory Details](#check-file-and-directory-details)
  + [Describe the Permissions String](#describe-the-permissions-string)
  + [Change File Permissions](#change-file-permissions)
  + [Change Hidden File Permissions](#change-hidden-file-permissions)
  + [Change Directory Permissions](#change-directory-permissions)
  + [Summary](#summary-file-permissions)
* [Project 2: SQL Query Filtering for Security](#-project-2-sql-query-filtering-for-security)
  + [Retrieve After Hours Failed Login Attempts](#retrieve-after-hours-failed-login-attempts)
  + [Retrieve Login Attempts on Specific Dates](#retrieve-login-attempts-on-specific-dates)
  + [Retrieve Login Attempts Outside of Mexico](#retrieve-login-attempts-outside-of-mexico)
  + [Retrieve Employees in Marketing](#retrieve-employees-in-marketing)
  + [Retrieve Employees in Finance or Sales](#retrieve-employees-in-finance-or-sales)
  + [Retrieve All Employees Not in IT](#retrieve-all-employees-not-in-it)
  + [Summary](#summary-sql-queries)
* [Skills & Tools Demonstrated](#-skills--tools-demonstrated)
* [Technical Competencies](#technical-competencies)
* [Key Learning Outcomes](#key-learning-outcomes)

---

## Overview

**Note: These projects are educational simulations designed to demonstrate Linux and SQL security operations skills**

This portfolio showcases practical command-line security operations using **Linux** and **SQL** to manage system security, investigate potential threats, and maintain secure file permissions. Each project demonstrates hands-on technical skills essential for cybersecurity analysts working in enterprise environments.

**Key Question Addressed**: How can we leverage command-line tools (Linux and SQL) to manage access controls, investigate security incidents, and maintain secure system configurations?

---

## 📊 Portfolio Quick Reference

| Metric | Value |
| --- | --- |
| **Total Projects** | 2 |
| **Primary Tools** | Linux Bash, SQL (MariaDB) |
| **Key Skills** | File Permissions, Access Control, Database Queries, Security Auditing |
| **Focus Areas** | Authorization Management, Security Investigation, Compliance |

---

## Skills Demonstrated

* Linux command-line navigation and file system management
* File and directory permission analysis using `ls -la`
* Permission modification with `chmod` command
* Understanding of Linux permission strings (10-character format)
* Managing hidden files and directories
* Principle of least privilege implementation
* SQL query construction and filtering
* Database investigation for security incidents
* Using AND, OR, and NOT operators in SQL
* Pattern matching with LIKE and wildcards
* Date and time filtering in databases
* Security audit trail analysis
* Incident response documentation

---

## 🔐 Project 1: Linux File Permissions Management

**Note: This is an educational simulation designed to demonstrate Linux file permissions management skills**

### Project Description

The research team at my organization needed to update file permissions for certain files and directories within the `projects` directory. The existing permissions did not reflect the appropriate level of authorization required for secure operations. I used Linux commands to audit current permissions and modify them to align with the principle of least privilege, ensuring the system remains secure while enabling authorized users to perform their work effectively.

---

### Check File and Directory Details

To begin the audit, I used the following command to examine the existing permissions:

```bash
ls -la /home/researcher2/projects
```

**Command Breakdown:**

* `ls` - Lists directory contents
* `-l` - Displays detailed information including permissions
* `-a` - Shows hidden files (those beginning with `.`)

**Output Analysis:**

The command revealed the following file structure in the `/home/researcher2/projects` directory:

| File/Directory | Permissions | User | Group | Other |
| --- | --- | --- | --- | --- |
| project_k.txt | -rw-rw-rw- | read, write | read, write | read, write |
| project_m.txt | -rw-r----- | read, write | read | none |
| project_r.txt | -rw-rw-r-- | read, write | read, write | read |
| project_t.txt | -rw-rw-r-- | read, write | read, write | read |
| .project_x.txt | -rw--w---- | read, write | write | none |
| drafts/ | drwx--x--- | read, write, execute | execute | none |

---

### Describe the Permissions String

The 10-character permission string provides detailed information about file access rights. Using `project_t.txt` as an example with permissions `-rw-rw-r--`:

**Position-by-Position Breakdown:**

1. **1st character (`-`)**: File type indicator
   * `-` = regular file
   * `d` = directory

2. **2nd-4th characters (`rw-`)**: User (owner) permissions
   * `r` = read permission granted
   * `w` = write permission granted
   * `-` = execute permission denied

3. **5th-7th characters (`rw-`)**: Group permissions
   * `r` = read permission granted
   * `w` = write permission granted
   * `-` = execute permission denied

4. **8th-10th characters (`r--`)**: Other (all other users) permissions
   * `r` = read permission granted
   * `-` = write permission denied
   * `-` = execute permission denied

**Interpretation**: The `project_t.txt` file can be read and modified by the user and group, but other users can only read it. No one has execute permissions since it's a text file, not an executable program.

---

### Change File Permissions

**Security Requirement**: The organization determined that the "other" category should not have write access to any files.

**Issue Identified**: The file `project_k.txt` had permissions `-rw-rw-rw-`, giving write access to "other" users.

**Solution Applied:**

```bash
chmod o-w project_k.txt
ls -la project_k.txt
```

**Command Breakdown:**

* `chmod` - Change file mode/permissions
* `o-w` - Remove (`-`) write (`w`) permission from other (`o`)
* Verification with `ls -la` confirms new permissions: `-rw-rw-r--`

**Result**: Write access was successfully removed from the "other" category while preserving read access for the user and group.

---

### Change Hidden File Permissions

**Security Requirement**: The archived file `.project_x.txt` should not have write permissions for anyone, but the user and group should be able to read it.

**Issue Identified**: The hidden file had permissions `-rw--w----`, giving write access to user and group, with no read access for group.

**Solution Applied:**

```bash
chmod u-w,g-w,g+r .project_x.txt
ls -la .project_x.txt
```

**Command Breakdown:**

* `u-w` - Remove write permission from user
* `g-w` - Remove write permission from group
* `g+r` - Add read permission to group
* Multiple permission changes separated by commas

**Result**: The file now has permissions `-r--r-----`, allowing read-only access for user and group, with no access for others. This protects the archived file from accidental modification while keeping it accessible for reference.

---

### Change Directory Permissions

**Security Requirement**: Only the `researcher2` user should have access to the `drafts` directory and its contents.

**Issue Identified**: The `drafts` directory had permissions `drwx--x---`, giving the group execute permission, which allows them to access the directory.

**Solution Applied:**

```bash
chmod g-x drafts
ls -la
```

**Command Breakdown:**

* `g-x` - Remove execute permission from group
* Execute permission on directories controls access to enter the directory

**Result**: The directory now has permissions `drwx------`, restricting all access exclusively to `researcher2`. The user already had full permissions (read, write, execute), so no additions were necessary.

---

### Summary: File Permissions

I successfully modified multiple file and directory permissions to align with organizational security requirements. The process involved:

1. **Auditing** current permissions using `ls -la` to identify security gaps
2. **Analyzing** the 10-character permission strings to understand existing access levels
3. **Modifying** permissions with `chmod` to implement least privilege principles
4. **Verifying** changes to ensure correct implementation

This systematic approach to permission management demonstrates practical application of Linux security controls, ensuring that users have appropriate access while protecting sensitive organizational data from unauthorized access or modification.

[🔝 Back to Top](#table-of-contents)

---

## 🔍 Project 2: SQL Query Filtering for Security

**Note: This is an educational simulation designed to demonstrate SQL security investigation skills**

### Project Description

As a security professional at a large organization, I investigated potential security issues by examining data in the organization's `employees` and `log_in_attempts` tables. Using SQL queries with various filters, I retrieved specific records to identify suspicious login activity, support security updates, and investigate potential threats. These queries demonstrate how SQL enables efficient investigation of security events and supports data-driven security decision-making.

---

### Retrieve After Hours Failed Login Attempts

**Security Context**: A potential security incident occurred after business hours (after 18:00). All failed login attempts during this time required investigation.

**SQL Query:**

```sql
SELECT * 
FROM log_in_attempts 
WHERE login_time > '18:00' AND success = FALSE;
```

**Query Breakdown:**

* `SELECT *` - Retrieves all columns from the table
* `FROM log_in_attempts` - Specifies the table containing login data
* `WHERE` - Begins the filter conditions
* `login_time > '18:00'` - Filters for attempts after 6:00 PM
* `AND` - Requires both conditions to be true
* `success = FALSE` - Filters for failed login attempts only

**Investigation Results**: This query returned all failed login attempts that occurred outside business hours, enabling the security team to identify potential unauthorized access attempts when most employees shouldn't be working.

---

### Retrieve Login Attempts on Specific Dates

**Security Context**: A suspicious event occurred on 2022-05-09. Login activity on this date and the day before required review.

**SQL Query:**

```sql
SELECT * 
FROM log_in_attempts 
WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';
```

**Query Breakdown:**

* `login_date = '2022-05-09'` - Filters for the suspicious event date
* `OR` - Returns records matching either condition
* `login_date = '2022-05-08'` - Filters for the day before

**Investigation Results**: This query returned all login attempts from both dates, allowing analysis of activity patterns leading up to and including the suspicious event.

---

### Retrieve Login Attempts Outside of Mexico

**Security Context**: The security team determined suspicious login activity did not originate from Mexico and needed to investigate all non-Mexico login attempts.

**SQL Query:**

```sql
SELECT * 
FROM log_in_attempts 
WHERE NOT country LIKE 'MEX%';
```

**Query Breakdown:**

* `NOT` - Negates the condition, excluding matching records
* `LIKE` - Enables pattern matching
* `'MEX%'` - Pattern matches 'MEX' or 'MEXICO'
* `%` wildcard - Represents zero or more characters

**Investigation Results**: This query excluded all login attempts from Mexico (whether recorded as 'MEX' or 'MEXICO') and returned attempts from all other countries, focusing the investigation on external threats.

---

### Retrieve Employees in Marketing

**Security Context**: The team needed to update specific employee machines in the Marketing department within the East building.

**SQL Query:**

```sql
SELECT * 
FROM employees 
WHERE department = 'Marketing' AND office LIKE 'East%';
```

**Query Breakdown:**

* `department = 'Marketing'` - Filters for Marketing department employees
* `AND` - Requires both conditions to be true
* `office LIKE 'East%'` - Matches any East building office (East-170, East-320, etc.)

**Investigation Results**: This query returned all Marketing employees in East building offices, providing the exact list of machines requiring security updates.

---

### Retrieve Employees in Finance or Sales

**Security Context**: Employees in Finance and Sales departments required a different security update than other departments.

**SQL Query:**

```sql
SELECT * 
FROM employees 
WHERE department = 'Finance' OR department = 'Sales';
```

**Query Breakdown:**

* `department = 'Finance'` - Filters for Finance department
* `OR` - Returns records matching either condition
* `department = 'Sales'` - Filters for Sales department

**Investigation Results**: This query returned all employees in either Finance or Sales, enabling targeted deployment of department-specific security updates.

---

### Retrieve All Employees Not in IT

**Security Context**: All employees except those in Information Technology needed a security update (IT had already received it).

**SQL Query:**

```sql
SELECT * 
FROM employees 
WHERE NOT department = 'Information Technology';
```

**Query Breakdown:**

* `NOT` - Excludes records matching the condition
* `department = 'Information Technology'` - Identifies IT department employees

**Investigation Results**: This query returned all non-IT employees, ensuring the security update reached everyone who still needed it while avoiding duplicate updates for IT staff.

---

### Summary: SQL Queries

I applied SQL filters to retrieve specific information from the `log_in_attempts` and `employees` tables for security investigations and updates. The techniques demonstrated include:

**Filtering Operators:**

* `AND` - Combined multiple conditions requiring all to be true
* `OR` - Combined conditions where any could be true
* `NOT` - Excluded specific conditions from results

**Pattern Matching:**

* `LIKE` with `%` wildcard - Searched for patterns in text data
* Enabled flexible matching for variations (MEX/MEXICO, East-170/East-320)

**Practical Applications:**

* Investigated after-hours failed login attempts
* Analyzed suspicious activity on specific dates
* Identified geographic patterns in login attempts
* Targeted security updates to specific departments and locations

These SQL skills enable efficient investigation of security events and support data-driven decision-making in cybersecurity operations.

[🔝 Back to Top](#table-of-contents)

---

## 🎯 Skills & Tools Demonstrated

### Linux Command Line

[![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat&logo=gnu-bash&logoColor=white)](https://img.shields.io/badge/Bash-4EAA25?style=flat&logo=gnu-bash&logoColor=white)
[![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat&logo=linux&logoColor=black)](https://img.shields.io/badge/Linux-FCC624?style=flat&logo=linux&logoColor=black)
[![File Permissions](https://img.shields.io/badge/File_Permissions-00666C?style=flat)](https://img.shields.io/badge/File_Permissions-00666C?style=flat)

### Database & Query Languages

[![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat&logo=mysql&logoColor=white)](https://img.shields.io/badge/SQL-4479A1?style=flat&logo=mysql&logoColor=white)
[![MariaDB](https://img.shields.io/badge/MariaDB-003545?style=flat&logo=mariadb&logoColor=white)](https://img.shields.io/badge/MariaDB-003545?style=flat&logo=mariadb&logoColor=white)

### Security Practices

[![Access Control](https://img.shields.io/badge/Access_Control-FF6B6B?style=flat)](https://img.shields.io/badge/Access_Control-FF6B6B?style=flat)
[![Least Privilege](https://img.shields.io/badge/Least_Privilege-4ECDC4?style=flat)](https://img.shields.io/badge/Least_Privilege-4ECDC4?style=flat)
[![Security Auditing](https://img.shields.io/badge/Security_Auditing-95E1D3?style=flat)](https://img.shields.io/badge/Security_Auditing-95E1D3?style=flat)

---

## Technical Competencies

| Competency Area | Specific Skills |
| --- | --- |
| **Linux Administration** | File system navigation, permission management, `chmod` command, hidden file handling, directory security, `ls` command options |
| **SQL Database Operations** | Query construction, filtering with WHERE clauses, pattern matching with LIKE, date/time filtering, logical operators (AND, OR, NOT), wildcard usage |
| **Security Controls** | Principle of least privilege, access control implementation, permission auditing, authorization management, security baseline configuration |
| **Investigation & Analysis** | Security incident investigation, failed login analysis, user activity tracking, compliance verification, data correlation |
| **Documentation** | Technical writing, step-by-step procedures, command explanation, security reporting, audit documentation |

---

## Key Learning Outcomes

**Linux Security Management:**

* Demonstrated ability to audit and modify file system permissions to implement least privilege
* Applied security best practices for file and directory access control
* Managed both visible and hidden files with appropriate security considerations
* Understood the relationship between permission types (read, write, execute) and security requirements

**SQL for Security Operations:**

* Constructed complex queries to investigate security incidents efficiently
* Used filtering operators to narrow down large datasets to specific security events
* Applied pattern matching to handle data variations and inconsistencies
* Demonstrated how database queries support security decision-making and incident response

**Professional Readiness:**

This portfolio showcases practical command-line and database skills essential for cybersecurity analyst roles. The projects demonstrate both technical proficiency with industry-standard tools (Linux, SQL) and understanding of security principles (least privilege, access control, incident investigation). These skills directly support real-world security operations including system hardening, threat investigation, and compliance management.

The combination of Linux file permissions management and SQL security querying demonstrates the breadth of command-line skills required for effective security operations. Both projects showcase systematic approaches to security tasks, attention to detail in permission configuration, and the ability to use technical tools to achieve security objectives.

---

## 🔗 Navigation

[⬅️ Back to Portfolio Home](https://github.com/TheCyberLeader) | [📧 Contact](mailto:m@riegrc.com) | [💼 LinkedIn](https://linkedin.com/in/mariezw)

---

[![](https://visitcount.itsvg.in/api?id=TheCyberLeader-linux-command-line-mastery&icon=0&color=0)](https://visitcount.itsvg.in)
