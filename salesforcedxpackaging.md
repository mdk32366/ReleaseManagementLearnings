### Salesforce Packaging

Metadata deployment methods:
 * Force.com Migration Tool
 * Change Sets
 * Unmanaged Packages
 
 Issues:  
 * Absence of an Immutable Artifact
 * Managing changes and upgrades
 * Production Orgs are a set of applied packages.  Can't be isolated or traced back to the app causing the issue.
 
 
 What's new?  Unlocked Packages.
 * Container for transporting Metadata to your org.
 * Organizes Metadata in the Org.
 
 Now Salesforce is a Source Driven Environment (wasn't before - Happy Soup)
 
 * Iterate Versions of Packages
 * Easy now to add/edit/remove components
 * Simplifies (really, for the first time) and Enables Continuous Development/Continuous Integration (CD/CI)
 * Manages Dependencies between packages
 * Metadata Organization
 
 Developing and Deploying an App using DX and Packages
 
 Local Workspaces - Set up VCS (Github) connected to an IDE (VS Code)
 
 * Developers (Declarative or Apex) spin up Scratch Orgs to develop a single feature.
 * Developer generates an Unlocked Package from the Scratch Org and checks it into Source Control
 * Release Manager ensures testing has taken place (likely using a script written for that purpose that automatically runs the test and reports coverage and test status) then deploys to UAT
 * Any changes that need occur to that package are managed by DX using the CLI and VCS to ensure the Unlocked Package contains that single Immutable Artifact that will eventually after being fully tested, will be deployed to the Production Org, where it will NOT be a part of the Happy Soup, but will be a Package.
 * Going forward, the fewer changes made outside of packages, the better in terms of maintainability.

Maintaining and Enhancing an App using Packages

* Branch from Source Control
* Develop in a Scratch Org
* Test new Version of the Unlocked Package with your one feature via CLI
* Push to UAT instance from Source Control using CLI
* Make changes, test, rinse, repeat
* Deploy new Version of the Unlocked Package to Production Org.

Migrating Existing Metadata into Packages
* Phased and Iterative
* Generally, the prod consists of Installed AppEx Apps and Unpackaged Metadata
* You start managing existing Metadata into small Unlocked Packages.
* Production environment evolves until you settle into mostly
  * Installed AppEx Apps
  * Newly organized Salesforce DX Packages
  * A little bit of Unpackaged Metadata (legacy stuff and odds and ends)
* Now able to source, version, and document each package.

App Hub
* Manage all packages in an Org.
* Currently Beta - Available in GA Summer of 19.

Package Types:
* Unlocked - editable packages
* Locked - not editable, except by certain developers.

### What is a Salesforce DX Project?

A Salesforce DX project is a local directory structure of your metadata in source format. It lets you develop and test with Salesforce DX tooling.

It contains configuration files for creating scratch orgs. It can contain data to be loaded into orgs for development or testing. It also contains tests that you rely on to validate your package.

When you use the CLI to create a new Salesforce DX project, it creates the project directory structure for you. When creating a project from scratch, many things are created for you. We create a base project configuration file. We create sample scratch definition files and directories for your tests and sample data sets. We also create a default “package” directory for your package source.

Remember, a package is a group of related code and customizations. You can test a package independently from other components in your org. You can release a package independently as well. The metadata components within a package can live in only one package at a time in the installed org.

At a minimum, the project manages the source for one package. That being said, if multiple packages get built and released together, you can organize these packages into a single DX project. Each of your packages aligns to a package directory defined in the project configuration file.

### How Scratch Orgs Rock the Development Process

Scratch orgs, built from your source and metadata, make it easy for you to build your app consistently over and over again. You only work with the source and metadata for a specific project. There’s no need to copy over things you don’t need (and frankly, we don’t recommend it). And because scratch orgs are temporary Salesforce environments, you can quickly spin up a new scratch org for each package or development project.

Once your VCS is set up, and your source is organized into packages, you’re ready to start a new development project. Open up your favorite IDE or text editor, then add or modify your source code. When you’re ready to see your changes in an org, you can create a scratch org.

After you create a scratch org, you still have some setup tasks to complete. You push all source from the project to the scratch org, set up permissions, and create or load any test data that your package requires.

While the IDE or text editor is available for programmatic (code-based) development, you can use the scratch org for declarative (point-and-click) development. This process is similar to what you do in your sandbox or production org. What’s different in the source-driven model is that you synchronize any development you did in the scratch org with your local project. This synchronization lets you commit changes made in the Setup pages alongside changes made in your local IDE.

Before you commit to your version control system, be sure to run tests. You can either use the same scratch org to run tests, or spin up another specifically for testing before you commit your source. This same pattern of spinning up scratch orgs for testing is a key part of an automated continuous integration system.

### Keep Your Project and Scratch Org in Sync

A key feature of Salesforce DX is that you can easily keep your project and scratch org in sync. So you can put away those sticky notes! You no longer have to jot down what you changed in your local file system, IDE, or editor, or what you changed in your org.

Salesforce DX tracks any change you make locally in the project and any changes you have made in your scratch org.

Before you push source changes to the scratch org, or pull changes to your local project, you can view a list changes you’ve made. That’s the power of the Salesforce CLI in action.

DX project format breaks down large source files to make them more digestible and easier to manage with a version control system. For example, Salesforce DX transforms custom objects and custom object translations into multiple files and directories. This source structure makes it much easier to find what you want to change or update. Smaller files in source control mean fewer merge conflicts during team development. Say bye-bye to messy merges!

Once you’re done developing, always commit your changes back into the VCS repo. Now you’re ready to test, build, and release with Salesforce DX.

### Testing and Continuous Integration Using Scratch Orgs
How you test, build, and release with package development is a shift from the current application life cycle.

If you’re currently using the change set development model, you move org changes (deltas) between development and test environments until those changes are released to your production org. At the end of the day, the “source of truth” is the production org. Even if you track changes externally in a version control system, you know with certainty that everything resides in your org.

But now you have options! In the package development model, the new and improved source of truth is your version control system. You use Salesforce DX projects to organize your source into package directories. Your end goal is to create packages using those directories that are versionable, easy to maintain, update, install, and upgrade.

You can use the Salesforce CLI during the entire package development life cycle.

![Package Development Lifecycle](https://res.cloudinary.com/hy4kyit2a/f_auto,fl_lossy,q_70/learn/modules/sfdx_dev_model/sfdx_dev_model_release/images/a9300b70e46ebe3c952c855be8d6d44f_package_dev.png)

When you’re ready to perform manual or exploratory testing of your development work, push your metadata into a separate scratch org designated for that purpose (1). You never pull anything from that org, as it’s only being used for testing or validation.

Continuous integration (CI) is about automating consistent test runs against every set of changes merged to your application (2). This important process ensures application quality before any corrupt change can get into your source repository.

Scratch orgs can be easily integrated into a CI process. The CLI can create scratch orgs, so scripting them into a CI flow is a piece of cake. You can populate the org with the appropriate version of the source repository and run tests on the specific change.

Unlike developer sandboxes, scratch orgs can be created throughout the day as opposed to a single refresh per day. You can delete a scratch org and create a new one quickly when the need arises. You can have multiple scratch orgs for different purposes. Scratch orgs give you a ton of flexibility with limited overhead.

When you’re ready for release testing or continuous delivery automation, you create a package version. Instead of using change sets to move changes between environments, you create and install package versions (3) in each testing environment. Once you complete testing, you install a package version in your production org.

### Continuous Delivery Using a Sandbox
For continuous delivery, you want to start testing the same process that you use when you deploy to your production org. In this use case, you want to test with the package you created in the build phase, and install it in a sandbox, which is the best representation of the production org. In a sandbox, you can replicate and test the steps you use to release to the production org.

### You Can Still Convert to Metadata Format to Build
Although package development is a great way to manage change in your happy soup of metadata, we still support the build and deploy process that parallels the current change set method. The Metadata API mdapi:convert and mdapi:deploy commands continue to handle build and deploy use cases.

At the start of a project, you can retrieve metadata source from an org and convert it to source format, then store it in a VCS. Once you’ve created and tested your app or customizations, you’re ready to create the deployment artifact. You can convert from source format back to metadata format, and then all your source is ready to deploy to a scratch org. You can deploy all the source, and the deploy operation takes care of updating the files that have changed. You can omit certain files from the deployment, and construct exactly what to deploy by modifying the package.xml file created during the convert process.

And the convert and deploy cycle continues. As you iterate on your DX project, you continue to convert source to metadata format for all release testing and continuous delivery use cases. Then, you can deploy it to the org using the Salesforce CLI. You can try out the deployment process using the CLI in the App Development with Salesforce DX module.

## Transitioning to Packages

Now that you understand how to use the new Salesforce DX tools and recognize their value, you’re interested in moving forward with it. So how do you begin? Your next steps depend on the complexity and maturity of your production org and the associated development processes. Here are some suggestions to help you get started.

Why Now Is a Great Time to Adopt Packaging
It used to be that packages were for partners who wanted to build and distribute applications on AppExchange. But now there’s a new package sheriff in town for enterprises and customers: unlocked packages. If you’re a Salesforce customer, contractor, consultant, or systems integrator, unlocked packages are for you!
These developer-controlled packages help mitigate some of the limitations of current technologies like change sets and the ANT Migration Tool. Unlocked packages provide a repeatable, scriptable, and trackable way to organize your work and manage change as you develop functionality.

And best of all, the Salesforce CLI and DX projects make creating unlocked packages a snap. You can install unlocked packages in any Salesforce environment: scratch orgs, sandboxes, trial orgs, and production orgs.

If you want to know more, check out the Unlocked Packages for Customers module.

### Assemble the Team
Perhaps you’ve heard the expression “No person is an island.” This same adage applies to teams. In many companies, your Salesforce production org has many stakeholders. As you get ready to embark on the journey of package development, one of the first tasks involves how to untangle your org. Before you begin, it’s important to include the right people.

Package Development Readiness provides strategies for assembling your team in preparation for moving to package development.

### Look for Ways to Untangle the Org into Packages
Evaluate all aspects of your development process to look for possible ways to shift to the modular, package-based approach. Look for distinct applications in the production org that are separate from everything else. Do you have distinct teams that build and maintain these applications? If so, you can isolate those applications into their own packages. AppExchange has many great examples of standalone apps that follow this idea of isolating a set of source and metadata into a single package.

Sometimes, you don’t have a distinct application that you can split into a package, but you have distinct parts of your org that you’ve worked on over time. For instance, extensions to one of your core apps could be released as packages. You can isolate all of the extensions you make in customizing your company’s sales process into a single package. If you can isolate the metadata that is specific to these parts, you can use that to develop a package.

You can also look for teams who already, or would like to, build, and deliver separately from everyone else. Find teams who are looking for opportunities to be more agile and flexible. Or teams who want to separate their changes from the larger change management process in their production org. These teams can isolate their metadata and store them in their own package.

### Look Out for Shared Metadata
Along the way, ensure that you evaluate all potential packages for shared metadata components. You don’t want to inadvertently isolate shared metadata to a package owned by a specific team or application. If the metadata component is shared, we recommend that you organize those shared components into a single base package. In this way, you can ensure that all packages can reference the components in the shared base package. (Remember, metadata components can only live in one package at a time.)

Start Your Package Project
Once you’ve identified potential packages, you can use the Metadata API to retrieve the source related to your package. See App Development with Salesforce DX to walk through how you’d use the Salesforce CLI and your testing org to create a package.xml that identifies the components for the package. Once you retrieve the source in metadata format, convert it to source format.

Then, create a VCS repository for each package. From there, continue the separation process by building packages specific to those applications.
