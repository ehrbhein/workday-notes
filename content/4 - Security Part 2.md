---
tags:
  - learning
domain: 
type: 
environment: 
aliases: 
date_created: 2025-03-04
---
## Testing security changes

### Security testing tools

#### Reports for security testing

It's important to test your security configurations as you implement them. You can use the _Start Proxy_ task to act as another user before and after you make changes. This step ensures that the configuration changed the user's access as expected. When testing, use restrictive groups or workers. Testing with users that have limited access ensures their new access is from your configuration and not from an existing security group.

Other reports you can use for common testing scenarios

1. Test security group membership
	- With this report, you can ask if the user is in a specified security group and has access to a target instance.

2. Compare Permissions of two security groups
	- This report analyzes the permissions of two security groups. It generates a report you can use to change the permissions of a security group to match another security group.

3. Compare security of two worker accounts
	- Lists the role and security group assignments in common and exclusive between two workers. Only Workday-Owned, User-Based, Job-Based, Intersection and Aggregation security group types display.

#### Security policy change control

As a security administrator, you will likely receive two types of change requests: Changes that require activation and changes that don’t.

Types of change requests:
1. Changes that require activation
	- For changes that require activation, such as adding a security group to a domain or business process security policy, Workday has built-in change control. Workday creates an active timestamp (an audit event) every time you run the Activate Pending Security Policy Changes task. Security timestamps capture all changes made to domain and business process security policies.

		- Use the Domain Security Policies Changed within Time Range report to generate a list of domain audit events, including changes to security groups.
		
		 - Use the Business Process Security Policies Changed within Time Range report to generate a list of these business process audit events, including changes to security groups. 

2. Changes that do not require activation
	- Changing role assignments or membership in security groups does not require activation. 
		- Use the Security History report to find every security change for a specified organization and its subordinates. 
		
		- Use the Security History for User report to display a history of security changes for a worker.

It's important to put change control processes in place for both types of change requests, so that you can track who requested a change, when, and why. You can run the _View All Security Timestamps_ report to review all security timestamps and their comments. If a mistake was made and you need to revert back to a previous timestamp, you can do so from the _Activate Previous Security Timestamp_ task. When you activate a previous timestamp, any changes made to security policies after that timestamp will be pending, but not deleted.


#### Audit trails

When you’d like to view an audit trail, there are several ways to go about it, depending on the information needed.

1. View user or task or object audit trail

	This report allows you to enter specific user accounts, tasks, and business objects. You can enter multiple choices for any of these prompts. If you leave all prompts blank, you will get a report of all changes to all users, tasks, and business objects in the specified period.

2. Audit trail for user

	 To review an audit trail for a user, select the user’s Related Actions, then select Audits > View Audit trail.

3. Audit trail for security group

	To review an audit trail for a security group, select the security group’s Related Actions, then select Audits > View Audit Trail.

4. Audit trail - security
	This report displays all security-related changes during the period specified, either based on a specific user account or for all accounts.



**Testing:**

- use "Business Process Security Policies Changed within Time Range" report to review the security audit trail on a specified time range
- use "Compare Security of Two Worker Accounts" report to compare the security group of two workers.
- use "Signons and Attempted Signons" report to test and investigate for worker's account signons .

### Security troubleshooting tools


Two common troubleshooting scenarios:
1. worker can't access a task or report
2. worker can only view data for some instances rather than all instances they expect to view

Delivered reports:
1. View security for securable item - shows you the policy securing the given item. When running this report you can specify items secured to domains or business process type.
2. Security analysis for securable item and account - this report shows you which security groups a given user uses to access a given item.


#### Steps for security troubleshooting

When you know there's an issue, but aren't sure where to start, these steps can guide you. You can save or print a PDF version of these steps at the bottom of the page.


1. Use one of the _secura_ reports to find the security policy for a secured item.
2. Review the security policy to find the controlling domain or business process.   
    - What permission is required? 
    - What are the allowed security group types?
    - For role-based security, is the role assigned at the appropriate level?
    
3. Use the _View Security Group_ report to check a security group's configurations and membership.
    - Does the security group have the correct constrained or unconstrained context?
    - Does the group have the correct domain and business process security policy access?
    - Does the group have the correct domain View or Modify security permissions?
    
4. Use the _View All Security Timestamps_ report to ensure you activated the pending security policy change.
5. Use the _Worker Security_ report to review security for a specific user or users.
6. Use the _View User or Task or Object Audit Trail_ report to find details about a change.
    - Do they have the correct role assignments?
    - Do they have the correct security groups assignments?

**Notes:**

- Other than using "compare security of two worker accounts" report, you can also use "compare permissions of two security groups" report to inspect the permissions of between two SGs.


##### Reverting security timestamps

1. navigate to "view all security timestamp"
2. select the related actions of the 2nd timestamp of on the table.
3. Go to security timestamp > activate this timestamp



##### Audit security changes

1. navigate to " view user or task or object audit trail"














## Troubleshooting security issues







