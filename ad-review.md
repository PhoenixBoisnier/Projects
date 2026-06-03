# Active Directory Account Audit  
Enterprise-Scale Account Ownership, Activity, and Security Review

## Overview
This project documents a comprehensive audit of legacy and potentially shared Active Directory (AD) accounts. The audit began after identifying several accounts with non-expiring passwords and expanded into a full review of more than 500 accounts across the organization.

The goal was to determine ownership, activity status, security posture, and appropriate remediation for each account while minimizing business disruption.

---

## Challenge
During an investigation into suspected shared accounts, I discovered numerous legacy AD accounts with passwords set to never expire. This indicated a broader issue with account hygiene and prompted a full audit of the environment.

Key challenges included:
- Identifying account ownership  
- Determining whether accounts were active or abandoned  
- Coordinating with multiple IT and business teams  
- Ensuring remediation did not break critical processes  
- Documenting findings and updating internal records  

---

## Audit Approach

### 1. Prioritization of High-Risk Accounts
The first step was to identify and resolve any VIP or high-impact accounts.  
Actions included:
- Direct outreach to important end users  
- Enabling password rotation  
- Reinforcing password complexity requirements  

This ensured that the most sensitive accounts were secured immediately.

---

### 2. Determining Account Ownership
To classify the remaining accounts, I collaborated with IT teams to determine whether accounts were:
- IT-owned and operated  
- End-user operated  
- Shared across teams  
- Unassigned or abandoned  

This allowed the audit to be divided into:
- Accounts delegated to IT for investigation  
- Accounts requiring direct follow-up with field users  

---

### 3. Assessing Account Activity
A combination of automated and manual methods was used to determine whether accounts were active:

#### Automated Pre-Screening
A PowerShell script:
- Emailed unknown or unassigned account owners up to three times  
- Warned that accounts could be disabled if no response was received  
- Helped categorize accounts into manageable groups  

#### Manual Verification
For accounts suspected to be inactive, I checked:
- AD LastLogonDate  
- SIEM login events  
- Email gateway activity  
- One final email to potential owners  

If all indicators suggested inactivity, the account was flagged for deactivation.

---

### 4. Remediation and Monitoring
Inactive accounts were submitted in batches to the change control panel for approval.  
Once disabled:
- Systems team monitored for process failures  
- Help Desk was instructed to escalate any issues immediately  
- Account owners were notified to report any unexpected disruptions  

Out of approximately 150 disabled accounts, only one required reactivation over several months, confirming the accuracy of the audit.

---

### 5. Documentation and Long-Term Maintenance
For accounts that remained active, I collected:
- Number of users with access  
- Password rotation practices  
- Offboarding procedures  
- Updated AD user object descriptions  
- Ownership and point-of-contact information  

Remaining accounts were either:
- Onboarded into CyberArk (PAM) for secure rotation  
- Recorded as security exceptions in the risk management portal  

---

## Tools and Technologies
- Active Directory  
- PowerShell  
- SIEM log analysis  
- Email gateway activity review  
- CyberArk (PAM)  
- Internal change control processes  

---

## Outcomes
- Identified and reviewed more than 500 legacy or questionable accounts  
- Disabled approximately 150 inactive accounts with minimal business impact  
- Updated documentation and ownership records across multiple teams  
- Improved AD hygiene and reduced risk from shared or unmanaged accounts  
- Integrated remaining accounts into PAM or risk management workflows  

---

## Summary
This project demonstrates my ability to:
- Conduct large-scale AD investigations  
- Combine automation with manual verification  
- Coordinate across IT, Security, and business teams  
- Remediate accounts without disrupting operations  
- Improve identity governance and documentation  
- Strengthen enterprise security posture through systematic auditing  

