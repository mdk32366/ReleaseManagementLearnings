#### Dude, where's my permission?

When working with profiles and permission sets in the Metadata API (MdAPI) have you ever wondered why a permission didn't migrate as expected? Maybe this happened when you were using a client app built on top of the MdAPI—like the Force.com IDE, Force.com Migration Tool, or Change Sets—instead of using the MdAPI directly, but regardless, you thought the permission would be there in the target organization and it wasn't.

This issue comes up quite a bit. Our documentation explains what is and isn't supported in the MdAPI, but it doesn't account for what happens when you try migrating permissions and settings. And regardless of how it happened, you're left confused and with the time-consuming task of manually configuring permissions or settings in your target organization.

There is some good and some bad news when working with any of the client apps that are built on top of the MdAPI. The good news is that they typically handle the creation of any necessary MdAPI elements so you don't have to worry about missing an XML element or a metadata entry. The bad news is that they can hide the MdAPI implementation. However, you can access the MdAPI directly with the Workbench tool, which helps to eliminate much of the mystery.

Here are some basic rules of thumb when migrating permissions using Workbench or any other tool:

1. Focus on the permission rather than the container.

Working with profiles and permission sets is unlike any other metadata type in the MdAPI. First of all, profiles and permission sets may consist of up to 12 different sets of overlapping permissions and settings—some of which are only found in profiles, some only in permission sets, and some in both.

Rather than thinking of migrating permissions as two metadata objects, profiles and permission sets, think of it as 12 different metadata objects. For example, think about migrating user permissions or tab settings rather than the standard user profile or the API-enabled permission set.

The permissions or settings that can be migrated include:
1.  overview information
2.  user permissions
3.  object permissions
4.  field permissions
5.  page layout access
6.  record type access
7.  custom tabs access
8.  custom apps access (tabset)
9.  apex classes access
10. apex pages (Visualforce) access
11. IP ranges
12. login hours

2.  Each permission or setting may have individualized behaviors when migrating.

In other words, expect the unexpected. In general these behaviors are coupled into similar settings.

As a general rule, behaviors can be grouped:

1.  record types are similar and dependent on page layouts
2.  object permissions are similar and dependent on field permissions
3.  IP ranges are similar to login hours
4.  apex access, Visualforce page access, and user permissions are similar
5.  app (tabset) access and tab settings are similar

3. When retrieving permissions (such as in an Inbound Change Set), in some cases only ‘enabled’ permissions are retrieved, and in other cases *both* ‘enabled’ and ‘disabled’ permissions are retrieved.

This is important because retrieving disabled permissions provide some discoverability with the definitions but it also creates additional metadata for you to keep track of when deploying.

Both enabled and disabled permissions are retrieved for:

1.  user permissions
2.  object permissions (when at least read access is enabled)
3.  field permissions
4.  assigned apps
5.  apex class access
6.  Visualforce page access

Only enabled permissions are retrieved for:

1.  IP ranges
2.  login hours
3.  tab settings
4.  record type visibilities
5.  page layout assignments

4. Including only profile or permission sets in the package.xml retrieves standard permissions only.

Example of package.xml that includes only a permission set and profile:

     <types>
       <members>Basic</members>
       <name>Profile</name>
    </types>
    <types>
       <members>Simple</members>
       <name>PermissionSet</name>
    </types>

This retrieves the following permissions every time regardless of what else is defined in package.xml:

1.     user permissions (Permission Set Only)
2.     IP ranges and login hours (Profile Only)

5. To migrate a permission that’s tied to a custom piece of metadata (such as a custom object, custom field, or apex class) include the component in the payload (package.xml) along with the profile or permission sets.

Example: to migrate custom object (CRUD) permissions, you must include CustomObject in package.xml and by using the wildcard (*), you will receive all custom objects in the payload.

    <types>
        <members>*</members>
        <name>CustomObject</name>
    </types>
    <types>
        <members>Basic</members>
        <name>Profile</name>
    </types>

6. Including custom components in your package.xml, the retrieval will include all the metadata, plus permissions or settings, you need.

You will be able to retrieve:

all object permissions with at least read access enabled
all field permissions
some record type visibilities
some tab visibilities  

7. Some profile settings won't be retrieved unless you include additional metadata in your package.xml.

1.  To get all tab visibilities instead of just some, you need to include the CustomTab type
2.  To get all record type visibilities including their layout mappings, you need to include the Layout type
For example, the following package.xml ties everything to CustomObject:

    <types>
         <members>Basic</members>
         <name>Profile</name>
    </types>
    <types>
         <members>Account</members>
         <members>Foo__c</members>
         <name>CustomObject</name>
    </types>  
    <types>
        <members>*</members>
        <name>Layout</name>
    </types>
    <types>
        <members>Foo__c</members>
        <name>CustomTab</name>
    </types>

8. To migrate a permission tied to standard schema metadata (such as Account object, Account.Number field), you still need to include CustomObject type in your payload (package.xml) but you need to explicitly specify the standard object as a member. The wild card (*) on CustomObject does not include standard objects.

For example, to migrate standard Account object (CRUD) permissions, include a standard object explicitly defined as a member of CustomObject in package.xml.

As a result, to get standard object permissions and settings, you will need the following package.xml:

         <types>
        <members>Account</members>
        <name>CustomObject</name>
    </types>
    <types>
        <members>Basic</members>
        <name>Profile</name>
    </types>

9. Permissions may be dependent on other permissions or your deployment will fail with an error.

Object permissions may:

require other permissions within the same object (for example, edit requires read).
require permissions across objects (for example, read on Assets requires read on Accounts).
be required by user permissions (for example, Edit Case Comments requires edit on Cases). 

10. It's possible to use the MdAPI directly to retrieve and deploy metadata.

You can change metadata entries with more precision by changing actual MdAPI element values (for example, <enabled>true</enabled>) or by omitting elements from deployment.

You may encounter different behaviors depending on what you, or a MdAPI enabled tool like Force.com IDE, omit or leave out from your payload (package.xml).

11. Unlike retrieval, deployment of permissions should only require that the payload (package.xml) has both the profile and/or permission sets defined and the actual permission values from the .profile and/or .permissionset files in the payload.

To deploy explicitly enabled permissions, the actual custom metadata doesn't need to be in the zipped payload, nor the custom metadata defined in the package.xml. This may seem strange since you must include a custom object in package.xml when retrieving object permissions, but not when deploying them.

The primary exception to this rule is if the custom metadata doesn't already exist in the target organization. For example, if you try to migrate object permissions on the Foo__c object and the Foo__c object doesn't exist in your target organization, your deployment will fail.

12. If you omit an entire entry, Salesforce ignores that particular entry during deployment and retains the original value of that entry in the target organization.

For example, if you omit

   <userPermissions>
      <enabled>false</enabled>
      <name>ApiEnabled</name>
   </userPermissions>

from a .permissionset file, the deployment ignores the API Enabled permission and retains whatever the original value is in the target organization.

13. If you omit all of the permissions or settings but not the header (where header is defined as the permission metadata type without any permission values), we disable that particular permission or setting, overwriting the value in the target organization.

For example, when if you omit

         <enabled>false</enabled>

from

     <userPermissions>
     <enabled>false</enabled>
     <name>ApiEnabled</name>
  </userPermissions>

the deployment assumes that the API Enabled permission should be revoked or set to false in the target organization.

14. If more than one type of permission is contained (for example, field permissions have both read and edit), when you omit any one of the permissions or settings but leave others including the permission header, then the deployment disables that particular permission or setting.

For example, if you omit

        <editable>true</editable>
from

   <fieldPermissions>
      <editable>true</editable>
      <readable>true</readable>
      <field>Foo__c.Bar__c</field>
   </fieldPermissions>

the deployment assumes the edit field permission should be revoked or set to false in the target organization.

The exception to this is if it depends on another permission that is included (for example, omit read but leave edit enabled).

15. There are some notable exceptions to these deployment rules.


Defaults: exist in <applicationVisibilities> and <recordTypeVisibilities> and are ignored when omitted unless a different application or record type becomes default in the same deployment.
<tabVisibilities>: are ignored during deployment and the original value is retained in the target organization if you omit the <visibility> element.
<recordTypeVisibilities>: are ignored during deployment and the original value is retained in the target organization if you omit the <visible> element where the record type is the default.
Omission of the entire <recordTypeVisibilities> entry where <layoutAssignments> are included in the .profile file during deployment results in the <layoutAssignments> overwriting the target values but the <recordTypeVisibilities> entry being ignored on the target. 


As this blog posting illustrates, migrating permissions using the MdAPI isn't as straightforward as migrating an object or a field. Following these guidelines will hopefully help clarify why a permission may or may not have migrated successfully.
