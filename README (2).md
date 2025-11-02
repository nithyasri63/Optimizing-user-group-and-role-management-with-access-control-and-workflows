# 🧩 Optimizing User, Group, and Role Management with Access Control and Workflows

Project Overview

This ServiceNow project focuses on implementing a Role-Based Access Control (RBAC) system combined with an automated approval workflow designed for a project management environment.

The system manages two primary users:

Alice – Project Manager (PM)

Bob – Team Member (TM)

The solution ensures:

Only authorized users can modify designated fields

Task approvals are automated using Flow Designer

Accountability and progress tracking are maintained through structured access control

Team Details:

Team ID:NM2025TMID03787

Team Size:4

Team Leader : Kanimozhi M [822222104017]

Team member : Lekshika R [822222104011]

Team member : Tamizh Mullai K[822222104045]

Team member : Nithyasri K [822222104043]

🧠 Problem Statement

In a small project team, consisting of a Project Manager (Alice) and a Team Member (Bob), managing project tasks efficiently becomes difficult without proper role definitions and controlled access.
The absence of clear permissions and workflows leads to confusion in task assignments, progress tracking, and approvals.
This project resolves those issues by establishing defined roles, access control, and automated workflows within ServiceNow.

⚙️ Objectives

Define and enforce clear user roles

Create a custom task management table with restricted access based on roles

Automate task approvals using Flow Designer

Validate and secure data modifications through Access Control Lists (ACLs)

🧩 System Architecture
👤 Roles
Role	Description	User
project_member	Full CRUD access to all tasks	Alice
team_member	Can edit only Status and Comment fields	Bob
🗄️ Database Table — u_task_table_2
Field	Type	Description
Task Name (u_task_name)	String	Title of the task
Description (u_description)	String (Long)	Details about the task
Assigned To (assigned_to)	Reference → User	Assigned user
Status (u_status)	Choice	Task state – new, in_progress, completed
Comment (u_comment)	String/Journal	Feedback or progress notes
Priority (u_priority)	Choice	low, medium, high
Due Date (u_due_date)	Date	Task deadline
🧱 Access Control (ACL Configuration)
Field / Table	Operation	Role	Permission
Entire Table (u_task_table_2.*)	CRUD	project_member	Full Access
u_status	Write	team_member	Editable
u_comment	Write	team_member	Editable
Other Fields	Write	team_member	Restricted ❌

Result:

Bob can update only Status and Comment

Alice has full access (Create, Read, Update, Delete)

⚙️ Workflow — Flow Designer Setup

Flow Name: Task Table
Application: Global
Trigger: Record Created or Updated
Table: u_task_table_2

Trigger Conditions
Field	Operator	Value
u_status	is	in_progress
u_comment	is	feedback
assigned_to	is	bob.p
Actions

Update Record

Field: u_status

Value: completed

Ask for Approval

Approver: Alice

Approval Field: u_status

Waits until approval is received

After approval, the system automatically updates the task’s status to Completed and logs the action in Flow Executions.

🧪 Performance Testing
Test ID	Description	Expected Result	Status
T1	Bob updates task status to In Progress	Flow triggers update → Completed	✅ Pass
T2	Alice approves the task	Approval recorded; task finalized	✅ Pass
T3	Bob tries to edit restricted fields	Access Denied	✅ Pass
Result Summary

ACLs successfully enforce field-level restrictions

Flow automation performs as expected

Approval notifications appear in My Approvals (Alice)

Flow logs confirm successful execution

📋 Requirement Analysis
Functional Requirements

Alice creates, manages, and approves all tasks

Bob can view all tasks but edit only his assigned ones

Automated approval flow notifies Alice when Bob completes a task

Non-Functional Requirements
Category	Requirement
Platform	ServiceNow PDI
Security	Access Control Lists (ACLs)
Reliability	Validated via Flow Designer execution
Maintainability	Modular role and table structure
🧩 Project Design
🏗️ System Layers
Admin → Configuration  
↓  
Alice (PM) → Task Management + Approval  
↓  
Bob (TM) → Status & Comment Updates

🔄 Workflow Logic
Trigger: Task updated → Status = In Progress  
↓  
Action 1: Update record → Status = Completed  
↓  
Action 2: Ask for approval → Approver = Alice  
↓  
Alice approves → Task finalized

🧠 Ideation Summary
Phase	Description
Problem Definition	Lack of role-based workflows in project management
Brainstorming	Introduced ACLs and automated approvals via Flow Designer
Prioritization	Focused on role clarity, automation, and traceability
🧩 Requirement Matrix
User	Task	Create	Read	Update	Delete	Approve
Alice (PM)	All Tasks	✅	✅	✅	✅	✅
Bob (TM)	Assigned Tasks	❌	✅	✅ (Status & Comment only)	❌	❌
Conclusion

This ServiceNow-based project successfully demonstrates the power of role-based access control and workflow automation in a project management setting.

Bob (Team Member) can only update task progress and comments.

Alice (Project Manager) reviews, approves, and finalizes tasks.

Flow Designer automates status updates and approval cycles.

ACLs ensure security, integrity, and accountability at every step.