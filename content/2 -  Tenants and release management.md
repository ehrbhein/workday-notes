## Workday tenants introduction

`Environments = workday tenants`

### Tenant types


**Implementation environment/tenant**

- This environment contains your implementation tenant
- you'll access this environment as you go through the deployment or onboarding process with workday. It will be likely to have more than one implementation tenant
	- one for testing, one for training, and one for loading legacy data

**Production tenant**

- contains live instance of workday
-  workday will create a sandbox tenant in adjacent to the production tenant.


**Sandbox tenant**

- Replica of production tenant used for testing
- Since sandbox and prod is identical. We can test configurations, reporting, etc in the sandbox before moving to production.

**Sandbox preview tenant**

- Much like the sandbox tenant
- It's a copy of the data that's in your production tenant, and you can request to have the data refreshed.
- The difference with sb-preview is that it offers you a place to review any features that aren't yet in your production tenant.
- Allow you to preview features not yet available in workday.


Notes: 
1. all the tenant use the same software version. when a weekly service update completes, the update applies to all tenants.
2. 


### Setting up tenants

**WDSetup tenant**

1. Configuration data is loaded into WDSetup tenant. Much of this data is already pre-configured according to industry.
2. WDsetup jumpstarts deployment with industry data and loads into implementation tenant.
	- workday loads the setup tenant data (foundational data) into your implementation tenant. 
	- The system loads just enough data so that you can sign in.
	- Then, the user admin determine what industry-specific data you'd like to add to the load.
3. One you've pulled in your own data, nothing more is pulled from the WDSetup tenant.
	- This prevent overriding user specific data (your data).


**Gold tenant**

- Once decided on the legacy load and now ready to go live with first workday module. We'll start in a clean tenant.
	- Workday has a special temporary environment that's reserved just for moving your data to production.
	- In that environment, we'll have one gold tenant.
- Gold tenant starts as an empty instance.
	- Implementation consultants loads the agreed data to this gold tenant.
- Data review and confirmation process happens in this gold tenant.
- Then, after confirmation, workday moves that gold tenant into Production instance.

**Flow:**
```
- WDsetup tenant
  |_ Gold tenant
	  |_ Workday production
```

### Tenant requests

**Tenant refresh**
- Every week, Workday refreshes the Sandbox tenant with a copy of the Production tenant taken on Friday at 6:00 PM PT.

**Request refresh**
- Additionally, you can request a tenant refresh if you want to refresh an Implementation, Implementation Preview, or Sandbox Preview tenant during a tenant maintenance window. Request the refresh at least one day before you need it for an important event, such as a coordinated testing effort or a training class. You can request the tenant refresh through the Workday Community.

**Exempt refresh**
- There may be situations where you want to postpone your Sandbox tenant refresh. For example, if you're still in the process of testing new configuration changes or new reports, you might want to keep the current Sandbox for another week until you complete your testing. Workday only allows up to two consecutive exemptions. Otherwise, the Sandbox tenants are refreshed in the third week.

#### Before a tenant refresh

There are authentication implications when Workday refreshes a tenant, either at your request, or automatically at the start of the release preparation window. 

Workday recommends that you save the non-SSO redirect URL of the affected tenant. This important step ensures that you're still able to access the destination tenant after it's been refreshed. 

Review the tabs below for specific considerations for both regular users and security administrators of your tenants.

**Regular users (Non-security administrators)**

- Confirm with your security administrator whether you'll sign in to Sandbox Preview using your Workday username and password or SSO (single sign-on). 
- If you use your Workday username and password, verify that your credentials work before the refresh. (They're separate from your SSO credentials). 
- If you use SSO, your security administrator will review settings before the refresh to avoid a lockout.

**Security administrators**

- To prevent tenant lockouts with SSO, sign in to your Production tenant and review the configuration in the _Edit Tenant Setup - Security_ task, specific to the Sandbox environment as shown in the image below. 
- For the Redirection URLs section, configure the local redirect URL for your Sandbox Preview environment and select the Preview Only checkbox.
- If you use SSO to sign in to Sandbox Preview, we recommend having a separate line in the Redirection URLs section for Sandbox.
- Workday recommends having a separate rule, or condition with a rule, specific to security administrators to ensure they can always sign in using their Workday username and password.
- Review the authentication policy configured in your Production tenant for your Sandbox and Implementation environments.

##### After a tenant refresh

If anyone is unable to sign in to the tenant, the security administrator should review the _Signons and Attempted Signons_ report. This report provides the reason for the failed sign in and corresponding solutions.

| Issue                                                                                                                             | Solution                                                                                                                                                                                                                                                     |
| --------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| The Invalid for Authentication Policy column is Yes.                                                                              | Talk to your security administrator to review the authentication policy. You don't meet the conditions for the policy rule set defined by the security administrator for that security group.                                                                |
| The _Signons and Attempted Signons_ report indicates that Workday credentials are incorrect, either invalid username or password. | Talk to your security administrator to confirm your credentials or reset your password.                                                                                                                                                                      |
| There's no record on the _Signons and Attempted Signons_ report that you tried to sign in, and you're using SSO.                  | Your security administrator needs to check the redirect URL in your Sandbox Preview tenant to ensure they've entered the correct Preview URL and selected the Preview Only checkbox.  <br> Confirm that you should use SSO credentials for this environment. |

### Proxy access

You can configure proxy access policies to allow certain users to proxy in as other users, for testing purposes. Proxying saves time when testing (e.g., business processes, reports) since you do not have to sign in and out as multiple users. You can create, edit, view, and delete proxy access policies in any tenant environment except Production.

Once configured, authorized users can access the _Start Proxy_ task and select a target user to proxy in as. The proxy user can act on behalf of the target user, performing all tasks that the target user can access.

Stop a proxy session with the _Stop Proxy_ task. If you want to proxy in as a second worker, you can start a new proxy without stopping the first one.

**Some key points and restrictions to keep in mind around proxy access:**

- You can only configure one policy per environment. However, one proxy access policy can apply to multiple environments.
- Proxy access policies apply to **non-production environments only**.
- You can only configure unconstrained security groups with proxy access.
- A user cannot start a proxy session when acting as a delegate.
- There is no support for connecting to another service when in proxy mode, including: integrations, scheduled reports, email, and more.

> 💡**While you can only configure one proxy access policy per environment, you can configure multiple rules to control the permitted access.**

#### Proxy access policy order

Workday assesses the rules in order, and stops when it finds an applicable security group (one that the target user belongs to) in the On Behalf Of column. Therefore, it's very important to consider the order of the rules. Put the most restrictive rule at the top of the list and the least restrictive at the bottom. An example of a proxy access policy is shown in the image below.


## Release cycles and update planning

### Workday updates and releases

All Workday customers are on the same version, enabling us to deliver functionality at a rapid pace, while being mindful of your busy schedules and critical business processes.

Workday delivers product features and services in two ways:
1. Weekly - Weekly service updates occur within the weekend maintenance window and deliver timely fixes and enhancements that are less impactful to customers.
2. Bi-annually - Feature releases occur twice per year (March and September) and contain a significant number of new features and functionality that may require uptake.

During the feature release cycles, Workday delivers new features and functionality to preview tenants. This typically occurs five weeks in advance of the feature release, if not earlier. At the end of the preview window, Workday delivers the feature release to all tenant types on the same date.

⭐ The primary communication tool for all updates to Workday is the Workday Community, a portal for customer information and communications. The Workday Community provides detailed roadmaps, schedules, update and release notes, announcements, webinars, and documentation associated with changes delivered in updates.

### Release timeline

1. Before release prep window
	- To prepare for the release window, Workday recommends that you review the strategy roadmaps in the Workday Community, for your organization's industry and for the product areas you have implemented. These roadmaps provide insight on what's to come for a product, feature, functional area, or industry. 
	- Additionally, during this time, you'll want to assemble your project team, a project plan, and test plans. The Release Preparation Center in the Workday Community offers many resources to help you prepare, including sample checklists to outline some of the activities you'll need to perform prior to each feature release. You can use these checklists as a starting point and from there, create your own test plans. 
	- Lastly, review the _What's New in Workday_ report and the What's New posts in the Workday Community. At times, features may have delays. Take this into consideration when planning your testing.

2. Release prep window
	New functionality from the feature release is available in your Sandbox Preview and Implementation Preview tenants five weeks before the feature release. During this window, Workday recommends that you:
	
	- Test the new features.
	- Check _What's New in Workday_ report and any corresponding What's New posts.
	- Check for any delivery date changes.
	- Communicate with Workday to resolve questions and issues.
	- Plan to communicate changes to your Workday users as needed.
	
	Additionally, if you're planning to validate your Production tenant before releasing it to your organization, you can lock your tenant before the Feature Release Service Update begins.

3. During feature release
	When Workday releases new functionality to your tenants, it's time to complete the following:
	
	- Review any feature delays and complete any required configurations that you previously configured and tested in your Preview tenant.
	- If you previously locked your tenant for validation, at this point you need to unlock your Production tenant.
	- Continue to explore new features and functionality. Additionally, review any retired functionality references.
	- Support the release by monitoring and addressing any issues.

4. After feature release
	After the feature release, focus your attention on the following: 
	
	- Review your project plan and test plans. Capture any feedback that will help you improve as you prepare for the next feature release. 
	- Identify the test scenarios that you want to keep for future release testing, and decide whether to add or remove additional scenarios.
	- Continue to support the release by monitoring and addressing any issues that come forward from the previous release.

5. Ongoing adoption
	 To continue to maximize your value in Workday, consider a plan forward for ongoing adoption. The Workday Community covers many additional resources that are available to assist you in these efforts, including:
	
	- Customer Success Series: Feature Adoption Blueprint
	- Adoption Kit



## Tenant management and setup

### Tenant management

A tenant is a unique instance of Workday that contains a set of data in a logically separated database. Workday secures each tenant through password-controlled access. You can configure your tenant to meet your business needs. 

Each tenant has a unique URL and is associated with specific IP addresses and DNS names. Your tenant URL includes the tenant base name, which you can customize. Once set, you can't change the tenant name. 

Workday performs most of the tasks that support your tenant. However, you can:

- Customize the Workday sign-in page.
- Establish the default settings for a variety of data formats.

#### Edit tenant setup tasks

Workday provides several tasks for configuring specific areas of your tenant. You can access these tasks individually by using the full task name, such as _Edit Tenant Setup - Business Processes_ or _Edit Tenant Setup - Financials_. Alternatively, you can access any _Edit Tenant Setup_ task by navigating to the _Tenant Setup_ report and selecting a tenant setup task. 

**Step 1: Edit tenant setup - global**

Manage tenant-wide language and location settings in these areas:

- Languages
- Personal Data for Workers
- Frequently Used Values
- Other


**Step 2: Edit tenant setup - search**

The Common search category is the default search category for your tenant. You can select a different default search category from the prompt, but you can't select All of Workday as the default search category for your tenant.

You can use the _Change Preferences_ task in order to select All of Workday as your preferred search category.

Additionally, you can configure search synonyms to replace the Workday term. For example, you can define synonyms such as "Vacation" or "PTO" for the Workday term, "Time Off". Afterwards, when you search for the terms "Vacation" or "PTO", Workday tasks and standard reports with "Time Off" in the title display in the search results.

**Step 3: Edit tenant setup - system**

Manage tenant-wide settings for Workday in these areas:

- Information
- Default Values
- Time Zone Configuration
- Name & Address Display
- System Setup
- Activity Stream Settings
- Mobile
- Location Services
- Inbox Settings
- Customer Central
- User Activity Logging
- User Feedback
- Media Settings

**Additional Edit Tenant Setup tasks include:**

- Edit Tenant Setup - Assistant
- Edit Tenant Setup - Business Processes
- Edit Tenant Setup - Financials
- Edit Tenant Setup - HCM
- Edit Tenant Setup - Integrations
- Edit Tenant Setup - Notifications
- Edit Tenant Setup - Payroll
- Edit Tenant Setup - Recruiting
- Edit Tenant Setup - Reporting and Analytics
- Edit Tenant Setup - Security
- Edit Tenant Setup - Student

##### ID definitions and sequence generators

ID definitions specify the format of identifiers (IDs) that Workday generates for different business objects and fields. A sequence generator identifies people, positions, incidents, or fields with a number that is unique within a specified time period. Workday provides two types of sequence generators and their corresponding tasks:

| Create ID Definition / Sequence Generator task                                                                                               | Create ID Definition / Gapless Sequence Generator task                                                                             |
| -------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| Sequence generators that generate sequences that are unique, but might have numbering gaps.                                                  | Sequence generators that generate sequences that never have gaps.                                                                  |
| These sequence generators allow you to restart the sequence at the beginning of the day, month, or year, and at a specified sequence number. | When using these sequence generators, you can't select a restart period or overflow behavior.                                      |
| When using sequence generators, be sure to specify a behavior for instances when the sequence overflows.                                     | You might use this type of sequence generator when it's important to maintain the sequence, such as when your auditors require it. |

You can create as many ID definitions and sequence generators as you need. The _Edit Tenant Setup_ tasks allow you to select default ID definitions for Workday to use for invoices, orders, receipts, employees, beneficiaries, and so on. If an ID definition in not selected, Workday generates its own internal ID codes.

> [!INFO] Info
> You can create both sequence generator types, but Workday determines which types you can specify on an _Edit Tenant Setup_ task for a given situation. For example, you can't use gapless sequence generators for spend authorizations, but you can use them for miscellaneous payment requests on the _Edit Tenant Setup - Financials_ task.

Fields on workday sequence generator:
1. Increment sequence id by - Enter a nonzero value by which the sequence number increases each time Workday generates a new ID.

2. Padding (Optional) - 1. Use Padding to indicate the number of digits present in the sequence.
    Enter a large enough padding value so that the sequence doesn't encounter an overflow condition. Workday recommends a padding value of at least 6. 
	
    If the sequence exceeds the number of digits you enter here, the Sequence ID Overflow Behavior setting determines the next number in the sequence.
    
    If you enter a zero, or don't enter a value in the Padding field:
    - The sequence number increases indefinitely.
    - Workday populates Sequence ID Overflow Behavior with Automatically Ignore Padding.

3. Sequence Id format
	Specify a format for the identifier. You can use any text string with these four special variables: `[seq], [day], [month]. [year]`.
	The date-generated values represent the date when the task generates the ID.

4. Time based reset settings (optional)
	To reset the [seq] value to an initial value at regular intervals.

5. The time interval when the [seq] variable starts again at the Restart Number value.
    
    The options are Day, Month, or Year; leave the field blank if you want [seq] to be unique within this ID definition. Days start at 12:01 AM. Months start at 12:01 AM on the first of that month. Years start at 12:01 AM on the first of January.
    
    If you set a Restart Frequency and set up Workday to continue a sequence from a different ID generation system, you must set:
    
    - A valid Last Date Used.
    - A nonzero Last Sequence Used.

6. Restart number
	The value you want [seq] to start at when you first use the ID definition, and every time the sequence restarts.

7. Time zone usage
	An alternate time zone on which to base the interval specified in the Restart Frequency field.
	
	This value doesn’t indicate when the ID generation actually occurs. Workday uses this value only to localize the date to the specified time zone. If you don’t specify a time zone, Workday uses Pacific Standard Time/Pacific Daylight Time (PST/PDT).

8. Sequence generator maintenance
	These fields aren't available for gapless sequence generators.

9. Sequence ID overflow behavior
	Configure how you want Workday to handle potential sequence identifier overflow conditions. Options include: 
	
	- **Allow Overflow with Truncated Sequences:** Workday truncates the most significant digit of the sequence number when an overflow condition occurs. Example: With a Padding value of 3, the next sequence number after 999 is 1000, but the 1 gets truncated, so the next value displays as 000.
	- **Automatically Ignore Padding:** The sequence generator ignores the Padding value when an overflow condition occurs. Example: With a Padding value of 3, the next sequence number after 999 is 1000.

10. Override tenant setup overflow notification group
	Select security groups for this sequence generator that will override the default overflow notification security groups.

11. Last sequence id (Optional)
	If you previously used a different ID generation system and you want Workday to continue that sequence.

12. Last sequence used
	The last sequence number used in your previous ID generation system.

13. Last date used
	The date on which your previous ID generation system generated the last sequence number.



### Workday accounts

You can manage certain settings for specific Workday-managed accounts. Examples include:

- Changing the account password of a user.
- Resetting the enrolled passwordless sign-in credentials for an account.
- Exempting a user account from multifactor authentication.
- Resetting the multifactor authentication configuration for a user.

Different options available for managing workday accounts
1. Individual user - Individual users can reset or change their Workday password based on the conditions set, using the _Change Password_ task. Users must know their current password in order to change it.
   
   Additionally, users can update some of their information using the _Change My Work Contact Information_ task.

2. Administrators - Administrators can set up rules to specify how Workday constructs usernames for accounts that Workday manages, using the _Maintain User Name Rules_ task. Usernames must be unique.  Additional rules can be created to resolve duplicate usernames.
   
   Additionally, those with security access can use the _Edit Workday Account_ task to manage username changes and password settings for individual Workday accounts.

#### Definite password rules

Workday enables you to set tenant-wide password rules and configure how Workday users can reset or change their passwords. However, these steps don't apply to accounts managed by delegated authentication or third-party identity providers that rely on single-sign-on. For these types of accounts, you must manually manage passwords for accounts that sign in using passwordless sign-in. This is because users need to sign into Workday with their password to set it up. 

You can configure tenant-wide password rules for accounts that Workday manages. Then, users must comply with these rules when they change or reset their Workday password. These rules apply only to permanent passwords, not temporary passwords. Workday maintains two different sets of password rules:

1. A set that applies to users who process Payment Card Industry (PCI) information. Configure the rules on the _Maintain Payment Card Industry Password Rules_ task.
2. A set that applies to all other users in your tenant. You configure those rules on the _Maintain Password Rules_ task.

#### Configure password reset

You can only change passwords for active accounts. You can configure how users can reset and change their passwords for Workday accounts. Workday accounts are accounts that Workday manages. 

Workday can recover forgotten passwords for implementer accounts if the Workday account has the required contact information. Workday rejects password resets for a user when:

- The account is currently disabled or expired.
- The information the user enters doesn't match the information stored in Workday. (If the user name is valid, Workday sends an email to the primary email address of the user, if provided. The email notifies the user of the failed reset attempt.)

#### Contacting Workday Support

In some cases, you may need to contact Workday Support to have Workday passwords reset. 

**Criteria**

In order for Workday Support to assist in this request, the following criteria must be met:
- A Named Support Contact (NSC) who is an active employee for the company the case is logged against must approve the request.  
    Note: We'll explore the role of the NSC in more detail later in this module.  
- All users must be unable to sign into the tenant or, all users with administrative rights to change the security settings in a tenant can't sign in.
- The Forgot Password? or the Change Password link on the sign-in page doesn't work for users.
- If the customer has an active project, no implementer accounts are enabled, or the implementers aren't available to make the changes.
- The target user account (the account for whom the password is being reset) has sufficient security rights to make necessary changes to the tenant once they can sign in.

**Restrictions**

Keep in mind that there are some restrictions, including:
- Workday Support can't make changes to a user's security.
- There may be cases where a user can access the tenant, but is unavailable. In order for Workday Support to assist, the user must be unavailable for an extended period of time (such as on leave or vacation, rather than out for lunch).
- Workday Support cannot reset your single-sign-on credentials. 
- Workday Support is also limited in the type and number of changes performed in a single request. Therefore, they would only be able to reset one account's password instead of several at once. Additionally, they wouldn't be able to update an account password and disable the active Authentication Policy within the same request.



## Testing and support

### Testing

It's critical for customers to complete thorough testing of their configuration and requirements within the Workday System. Testing can take place at different times and for different purposes. During deployment, your consultants are heavily involved in testing. This is to ensure that your systems are set up to meet your company's requirements. If applicable, you may be asked to participate in testing during this time. However, your role will be more involved in testing once you are live with Workday.

**Testing reasons:**

1. Regression testing for upcoming releases.
2. Front-line support for triaging system issues.
3. Configuration testing for tenant-wide changes.

#### Testing process

1. Execute test scenarios
	Any time a customer is using or preparing for new functionality, it's important to identify scenarios that would be important for testing purposes. 
	
	This can include executing test scenarios or test scripts. Test scenarios include high-level direction to complete certain tasks, without additional instructions. Test scripts provide a specific task to complete, along with step-by-step instructions. Both are used for the purpose of confirming that system configuration is working as intended.

2. Record test results
	While testing, it's important to ensure you thoroughly document your results. Customers may use different tools and templates to track their testing, but regardless of what you use it's necessary to provide all relevant details. Additionally, you should be familiar with your company's testing statuses and what each mean. This will ensure that you record your results correctly. During your testing, some of the specifics that may need to be captured include:
	
	- Who was the test employee? 
	- Who were you signed in as?
	- Were you able to complete the task? 
	- If not, what happened?

3. Analyze, resolve, escalate issues
	Depending on your testing results, some scenarios may need to be tested several times before being completed. In order to resolve any issues uncovered during testing, you may need to research in the Workday Community. This may include checking Customer Alerts for any known issues. From there, work with your support team on what additional steps should be followed.

4. Adjust configuration
	If a resolution is provided, testing still continues. If necessary, you will adjust configurations and test any fixes or provided solutions to ensure all issues have been resolved. Additionally, if research alone reveals what corrective action to take, you will continue to work with your support team and whoever may be responsible for fixing the known issue. Audit reports may be helpful during this step to confirm success and completion.

5. Adjust scenario as needed
	Confirm if there are additional scenarios or revisions to existing scenarios that still need to be tested.

#### Regression testing

The purpose of regression testing is to verify that configuration, security, business processes, reporting, and integrations continue to behave as expected following a Workday release or when introducing new functionality.

- Who: Once live on Workday, customers will create their own specific regression testing scenarios to complete.
- What:  Customers will test any areas that are potentially affected by an upcoming feature release to ensure it performs correctly.
- When: For the purpose of testing new features included in a Workday release, regression testing will take place at least twice a year during the release preparation window.
- Where: Customers should use their Sandbox Preview tenant (recently refreshed with Production) to complete their regression testing for a feature release.
- how: Though processes vary between customers, you will generally use your own test plans, test team, and test tracking tools decided on by your organization.

#### Test scenario templates


As reviewed in the process above, customers may use different tools and templates in order to track their results from testing. (For example, a spreadsheet similar to the image below.)

| function area | test type | test scenario description | expected result | tester | status | notes | 
| ------------- | --------- | ------------------------- | --------------- | ------ | ------ | ----- | 
|               |           |                           |                 |        |        |       | 


- Functional area: The area of Workday (aligned with the Workday solutions), that the test scenario is related to.

- Test type: The purpose for testing. Different roles complete different types of testing. However, common testing types may include: Regression / feature release, Unit, End-to-End testing, and more.

- Test scenario description: A high-level direction or action statement to complete a certain task. This does not usually include step-by-step instructions or screenshots. For example: Hire an existing Pre Hire and validate that the Employee ID was created correctly.

- Expected result: What should happen as a result of completing a test scenario. For example, if the test scenario is to start an international assignment for an employee, the expected results may be that the process, notifications, and any approvals are aligned and completed with the current process in the Production tenant.

- Tester: The person completing the test. If applicable, could include who the tester was signed in as or proxied in as when testing.

- Status: Update/results for the test scenario. May include examples such as:
	- **Not Started**: Awaiting start
	- **In Progress**: Started; pending completion
	- **Pass/Complete**: Met requirements
	- **Pass w/Issues**: Functionality met requirements; internal discussion needed
	- **Fail**: Did not meet requirements; requires fix/solution
	- **Retest/Repeat**: Remedy in place; ready for retest
	- **Canceled**: No longer needed
	- **Deferred**: Move to later test cycle
	- **On Hold**: Waiting on something in order to complete; require direction/support in order to proceed

- Notes: Any important notes applicable to testing. May include screenshots when applicable.

### Support

#### Workday support model

Workday aims to provide you with the best support and exceed your expectations. This is accomplished by:

1. Proactively identifying potential issues.

2. Quickly resolving issues before customers even need to submit a Workday Community support case or contact Workday Support.

3. Alerting you when Workday identifies an issue and providing workarounds. They'll also update you as to when you can expect a resolution. 

4. Focusing on self-sufficiency. The Workday Community is a space to share ideas, ask questions, and provide feedback. You can collaborate with other Workday users to quickly get answers to your questions.

#### Support teams

Depending on your organization's requirements, your support team can operate under different models. However, all types of support teams will need to keep a similar area of focus that includes:

- Driving continuous feature adoption across Workday applications.
- Getting the most out of product innovations.
- Generating value for the organization it supports.
- Coordinating efforts around testing and troubleshooting



#### Customer support roles


1. Named Support Contact Admin (NSCA)
	The named support contact admin (NSCA or NSC Admin) is responsible for:
	
	- Maintaining contact management in the Workday Community, including adding/removing/reassigning roles.
	- Researching and triaging system issues.
	- Submitting and updating any and all cases within the Workday Community on behalf of your organization. 
	- Being available by phone for follow-up when submitting high severity cases.
	- Reviewing customer alerts within the Workday Community.

2. Community admin
	Community admins are responsible for approving and managing the Workday Community user access on behalf of your organization. This role consists of those in your organization who are not set up in support contact rules, but still need access to the information available in Workday Community. 

3. Read only named support contacts
	This role is a great option for those that don't need to submit cases in the Workday Community, but do need visibility to the cases submitted by the NSCA. Read only named support contacts can only view cases.


4. Security lead
	Your organization should have at least one person as the primary point of contact for security communications. This role is responsible for the following:
	
	- Partnering closely with the NSCA.
	- Understanding business requirements around security.
	- Acting as experts on tenant security setup.
	- Responding to any system incidents and network blocks.
	- Acting on security notifications.
	- Communicating and coordinating countermeasures related to your IT infrastructure and your Workday tenants.

5. Training coordinator
	Your organization can have up to two training coordinators. The training coordinator is the primary point of contact for all Workday training related information. They are responsible for:
	
	- Managing training credits on behalf of your organization. 
	- Assigning and managing Workday training courses in the Workday Learning Center.
	- Approving new users' learning accounts in the Workday Community.

#### Notifications

**Support notifications and alerts:**

Workday sends alerts when high impact issues are identified. These alerts include:

- Data center disruptions
- Known product defects
- Other changes to the global Workday customer base

Alerts are available on the Workday Community and are also sent through email to all named support contacts. (Note: If configured, you can also send alerts via text message.)

**Proactive support cases and notifications**

Sometimes Workday Support needs to inform a defined group of impacted customers regarding an issue that requires follow-up. When this occurs, Workday creates cases on behalf of the customer. These cases are available in the Workday Community and are also emailed to impacted customers' named support contacts.

If the issue does not require correspondence or follow-up, Workday sends the notification to specific customers': NSC Admins, Read-only NSCs, and Account team.


#### Workday maintenance

**Workday scheduled maintenance:** 

The Workday Community keeps an updated table that lists all planned maintenance windows. This includes monthly maintenance, quarterly maintenance, and feature releases. The table is planned out a year in advance, and includes maintenance types, dates, and tenant downtime. The example table below shows current information without including specific dates.

| Weekly Service Update                                         | Monthly Planned Maintenance                                        | Quarterly Planned Maintenance                                 | Feature Release                                               |
| ------------------------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------- | ------------------------------------------------------------- |
| Standard Maintenance                                          | Includes Weekly Service Update                                     | Includes Weekly Service Update                                | Production Tenants- Unavailable for a maximum of 3 hours      |
| Non-Production Tenants- Unavailable for a maximum of 12 hours | Production Tenants- Unavailable for a maximum of 7 hours           | Production Tenants- Unavailable for a maximum of 11 hours     | Non-Production Tenants- Unavailable for a maximum of 12 hours |
| Production Tenants- Unavailable for a maximum of 3 hours      | Non-Production Tenants-  <br>Unavailable for a maximum of 12 hours | Non-Production Tenants- Unavailable for a maximum of 16 hours |                                                               |

> 💡Workday sends notifications when the Production tenant becomes available before the scheduled end time.

**Workday status system notices**

When Workday conducts planned maintenance or encounters an unplanned service interruption, Workday displays system notices to inform end users.

#### Additional resources

- **Workday community**
	As a reminder, the Workday Community is the best resource for all things Workday related. The Workday Community allows you to:
	
	- Create and view tenant management requests and cases.
	- Access searchable content.
	- Validate your profile information.
	- View announcements, reports, content, and dashboards.

- **Service availability reports**
	Additionally, Workday generates monthly reports that summarize the activity in your Production tenant. These reports include metrics on your usage of the Workday Production tenant, tenant availability (uptime), and response time (performance). Information on how to request these reports is available within the Workday Community.
	
	- Usage Metrics Report
	- Service Availability Report
	- Service Response Time Report

- **Datacenter status**
	Workday houses Production, Sandbox, Sandbox Preview, and Implementation tenants in state-of-the-art data center regions designed to host mission-critical computer systems with fully redundant subsystems and compartmentalized security zones. 
	
	  
	The Trust Status and Maintenance (Datacenter Status) site in the Workday Community provides an at-a-glance high-level summary on the current health status of your Workday data centers. For each data center, information includes:
	
	- Details of upcoming scheduled maintenance.
	- A historical view of the past 24 hours of past 30 days.
	- Current health status of your Workday Data Center and Services.

#### Additional notes

- Workday recommends that all regression testing take place in Preview tenants (Sandbox or Implementation).
- Workday recommends that you regression test within the five week release preparation window to ensure that you can prepare end users for the changes.

