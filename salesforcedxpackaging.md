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
