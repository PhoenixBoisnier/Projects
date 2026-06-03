# Password Blocklist Automation  
Automated Weak Password Detection and Remediation Across Active Directory

## Overview
This project documents the multi‑stage development of an automated password blocklist enforcement system used to identify and remediate weak, breached, or predictable passwords across an enterprise Active Directory environment.

The system evolved through three major iterations, scaling from a small dictionary to an 84GB dataset containing billions of passwords. The automation enforced password hygiene, reduced organizational risk, and integrated with existing security workflows without disrupting business operations.

---

## Challenge
When I joined the security team, the organization was implementing a password blocklist solution. A security engineer had already created a script to collect NTLM hashes for all accounts and compare them against the *rockyou* dictionary. My responsibility was to build the automation that:

- Interpreted the output  
- Validated account state  
- Applied appropriate remediation actions  
- Scaled to increasingly large password datasets  
- Avoided business disruption  
- Preserved a positive user experience  

Over time, the dictionary expanded from a few gigabytes to 15GB, and eventually to 84GB, requiring new engineering approaches to handle file size, memory constraints, and performance.

---

## Solution Architecture

### Input Processing
The input to the automation was a CSV containing fields for accounts whose passwords appeared in the dictionary. Useful fields included:

- EmailAddress  
- SamAccountName  
- Enabled  

When querying AD, the script also retrieved:

- LastLogonDate  
- ChangePasswordAtLogon  

This allowed the automation to determine whether the user was active, whether their password had changed since the scan, and whether remediation was appropriate.

---

## Remediation Logic
The PowerShell automation performed the following steps:

1. Detect new input files.  
2. Read and validate user entries.  
3. Skip accounts that:  
   - Were disabled  
   - Had changed their password since the scan  
   - Appeared on the bypass list (service accounts, sensitive accounts)  
4. For eligible accounts:  
   - If Monday–Saturday: send a warning email  
   - If Sunday: force a password change at next logon  
   - If the account already required a password change and had not logged in for 21+ days: reset the password to a long, random string  

This logic balanced security with user experience.  
A small percentage of weak passwords were intentionally tolerated to avoid forcing users to reset passwords twice in one day.

---

## Iteration 1: Initial Automation
The first version implemented the full remediation workflow. During this phase:

- A logic bug was discovered that caused premature password resets.  
- The bug was fixed while adding support for detecting accounts using the default password.  
- The system stabilized and began reliably enforcing password hygiene.

---

## Iteration 2: Scaling to a 15GB Dictionary
The second version introduced significant engineering challenges:

- PowerShell and common text editors could not load the 15GB dictionary.  
- Memory constraints prevented simple processing.

To solve this, I wrote a Python script to:

- Split the dictionary into manageable chunks  
- Accept parameters for input file and output chunk size  
- Process large strings efficiently  

This approach allowed the organization to process the expanded dataset and immediately increased weak‑password detections to roughly 100 users per scan.

A custom chunk file was also introduced to capture predictable, organization‑specific passwords that users repeatedly attempted to use.

---

## Iteration 3: Scaling to an 84GB Dictionary
The third version expanded the dictionary to 84GB, containing billions of passwords. Enhancements included:

- Converting the Python splitter into a fully terminal‑driven tool for repeatability  
- Splitting the dataset into 164 files  
- Creating a Python search tool to locate specific passwords across all chunks  
- Optimizing the search by skipping irrelevant chunks based on sorted data  

This allowed the team to search billions of passwords in minutes and resulted in more than 500 additional detections of breached or weak passwords.

---

## Design Considerations

### User Experience
The automation intentionally avoided forcing users to reset passwords twice in one day.  
This decision reduced friction and prevented complaints that could jeopardize the project.

### Service Accounts
A bypass list was introduced to avoid disrupting business‑critical processes.

### Legacy Constraints
Password complexity enforcement was not initially possible due to legacy systems.  
This automation served as a compensating control until complexity could be enforced domain‑wide.

---

## Tools and Technologies
- PowerShell  
- Python  
- Active Directory  
- NTLM hash comparison  
- Large‑scale file processing  
- Custom automation workflows  

---

## Outcomes
- Successfully enforced password hygiene across thousands of accounts  
- Detected hundreds of breached or predictable passwords  
- Scaled from small dictionaries to an 84GB dataset  
- Improved security posture without disrupting business operations  
- Created reusable Python tooling for large‑file processing and searching  

---

## Summary
This project demonstrates my ability to:

- Engineer scalable security automation  
- Work with large datasets and memory‑constrained environments  
- Integrate PowerShell and Python to solve complex problems  
- Balance security requirements with user experience  
- Identify and remediate weak passwords at enterprise scale  

