Permission Sets and Profile Settings in Change Sets
Developers can use permission sets or profile settings to specify permissions and other access settings in a change set. When deciding whether to use permission sets, profile settings, or a combination of both, consider the similarities and differences.

REQUIRED EDITIONS
Available in: both Salesforce Classic and Lightning Experience

Available in Enterprise, Performance, Unlimited, and Database.com Editions

Permission sets available in: Contact Manager, Professional, Group, Enterprise, Performance, Unlimited, Developer, and Database.com Editions

Note

NOTE In API version 40.0 and later, when you deploy the output of a retrieval to another org, the metadata in the deployment replaces the target org metadata. In API version 39.0 and earlier, when you deploy your retrieved permission set output to another org, the deployment contents are merged with your current org data. 

For example:
In API version 40.0 and later, if your permission set contains the Manage Roles user permission and you deploy a metadata file without this user permission, Manage Roles is disabled in the target org. Or, let's say you have a permission set with edit access to fields in an object. If the change set fails to include these fields, the permissions are still carried over to the target org and these changes are deployed.

In API version 39.0 and earlier, if you deploy a metadata file without this user permission, Manage Roles remains enabled.
Keep in mind that by default, change set deploys enable dependencies. For example, if your change set contains a custom profile with Lead Conversion enabled, Lead object permissions are enabled.

BEHAVIOR	PERMISSION SETS	PROFILE SETTINGS
Included permissions and settings	
Standard object permissions
Standard field permissions
User permissions (such as “API Enabled”)

Note
NOTE Assigned apps and tab settings are not included in permission set components.

Tab settings
Page layout assignments
Record type assignments
Login IP ranges
User permissions
Included permissions and settings that require supporting components	
Apex class access
Visualforce page access
Assigned apps
Custom object permissions
Custom field permissions
Apex class access
Visualforce page access
Added as a component?	Yes	No. Profiles are added in a separate setting.

For Visualforce page access and Apex class access, always include supporting components in the change set.

Note
NOTE Login IP ranges included in profile settings overwrite the login IP ranges for any matching profiles in the target org.

SEE ALSO
Deploy Inbound Change Sets
Upload Outbound Change Sets
Change Sets Best Practices
