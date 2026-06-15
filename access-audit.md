# AD Group Membership Access Delta Calculation Automation  
Tracking Permission Drift in a Hybrid RBAC–ACL–DAC Environment

## Overview
This project documents an automation solution designed to detect and quantify access drift in an Active Directory environment that uses a hybrid access‑control model. In this model, job titles define the *intended* baseline access (RBAC‑like), AD groups serve as the *actual* enforcement mechanism (ACL‑based), and certain permissions are granted manually at the discretion of various teams (DAC).  

Because these layers operate independently, access creep and inconsistent permission patterns were common. This automation provides the visibility needed to understand how access changes over time and where discretionary assignments diverge from expected baselines.

---

## Challenge
The organization’s access model combined three competing mechanisms:

- **Title‑based baseline access (RBAC‑like):** Titles map to expected access patterns but are not authoritative.  
- **AD group membership (ACL):** Groups determine real access to systems, applications, and resources.  
- **Discretionary assignments (DAC):** Teams with sufficient rights can add or remove group memberships without centralized logging or governance.

This created several issues:

- No reliable way to determine when access creep occurred  
- No visibility into which titles were accumulating unexpected permissions  
- No mechanism to compare intended vs. actual access  
- No data to support governance improvements or role design  

To address this, I built an automation system that calculates deltas in group membership across titles over time.

---

## Solution Architecture

### 1. Baseline Generation
The automation was implemented in PowerShell and begins by collecting all user objects in the domain.  
On the first execution, the script:

- Enumerates all users  
- Records each user’s job title  
- Records all AD groups assigned to each user  
- Builds a nested hash map structure:  
  - Outer key: job title  
  - Inner key: group name  
  - Value: number of users with that title in that group  
- Exports the baseline to a CSV file  

This baseline represents the *intended vs. actual* access distribution at a point in time.

---

### 2. Delta Calculation
On subsequent runs, the script:

1. Loads the previous baseline into memory  
2. Builds a new nested hash map from current AD data  
3. Compares the two datasets  
4. Calculates deltas for each title‑group pair  
   - Positive values indicate increased access  
   - Negative values indicate decreased access  
5. Outputs the results to a CSV for review  

This delta file provides a clear, quantifiable view of how access has shifted since the last run — including discretionary changes that would otherwise go unnoticed.

---

### 3. Reporting and Distribution
The automation produces:

- Weekly deltas  
- Monthly deltas  
- Current title‑group totals  

These reports are delivered to the Information Security and Systems teams to support:

- Access governance discussions  
- Identification of unexpected or unauthorized permission changes  
- Detection of access creep  
- Validation of whether title‑based baselines match real‑world access patterns  

---

## Future Applications

### Role Design for RBAC
Because the automation calculates both totals and percentages of users with specific permissions, it can be extended to support role design.  
For example:

- If 95% of users with Title A have Group X, Group X is likely part of the intended role.  
- If only 2% of users with Title A have Group Y, Group Y may represent discretionary access or drift.

### Automated Access Cleanup
The script could be expanded to:

- Remove group memberships below a defined threshold  
- Enforce role‑based access patterns  
- Maintain an ACL bypass list for:  
  - Temporary access  
  - Testing  
  - One‑off exceptions  

This would allow the organization to gradually transition from a hybrid model toward a more structured and governable access‑control framework.

---

## Tools and Technologies
- PowerShell  
- Active Directory  
- Nested hash map data structures  
- CSV‑based state tracking  
- Scheduled reporting workflows  

---

## Outcomes
- Provided the first quantitative visibility into access drift across titles  
- Identified discretionary permission changes that previously went undetected  
- Enabled leadership to make data‑driven decisions about access governance  
- Created a foundation for future RBAC or hybrid access‑control modernization  
- Improved understanding of how permissions evolve across job roles  

---

## Summary
This project demonstrates my ability to:

- Engineer identity governance automation  
- Analyze and compare large AD datasets  
- Detect permission drift in hybrid RBAC–ACL–DAC environments  
- Build scalable, repeatable reporting workflows  
- Support long‑term access‑control modernization efforts  

