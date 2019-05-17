### [Salesforce DX Setup Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm)

#### 1.  Authorize DevHub in Salesforce Production Organization

Enable the Developer Hub (Dev Hub) in your Developer Edition, trial, or production org (if you're a customer), or your business org (if you're an AppExchange partner).

Enable Second-Generation Packaging (2GP) in your org so you can develop 2GP packages.


#### 2.  Add Users to the DevHub
If you want to include your team, you can add users to your Dev Hub org.
Ensure that your computers meet all system requirements.


#### 3.  Choose your IDE (VS Code has Salesforce DX Extensions built in)
Download Visual Studio Code and Install it.  Install Salesforce Extensions for VS Code extension pack.

#### 4.  Install Salesforce Command Line Interface (Salesforce CLI)
[Installation Documentation](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_install_cli.htm#sfdx_setup_install_cli)

[Windows Installer](https://developer.salesforce.com/tools/sfdxcli)

##### Verify Your Installation
Verify your Salesforce CLI installation and plug-in versions.
Run this command to verify the Salesforce CLI version:

    sfdx --version
    sfdx-cli/6.0.10-3713d7b alpha (darwin-x64) node-v8.6.0
    
Run this command to verify the Salesforce CLI plug-in version:

    sfdx plugins --core
    salesforcedx 41.2.0 (core)

This command returns a list of the other plug-ins installed in the CLI:

    sfdx plugins

The core salesforcedx plug-in is not included in the preceding list unless you’ve explicitly installed a newer version with the sfdx plugins:install command.

Run this command to return a list of the command families in the force topic:

    sfdx force --help

This command returns all the force commands:

    sfdx force:doc:commands:list
    
    
##### Disable Automatic Update of the CLI and Plug-In
When you run a command, Salesforce CLI checks to see if you have the latest version. If not, the CLI automatically updates itself and the salesforcedx plug-in. You can disable this automatic update with an environment variable.
To remain on the current version of the CLI and disable automatic updates, set the SFDX_AUTOUPDATE_DISABLE environment variable to true. See the help for your operating system for instructions on how to set environment variables.

##### Setting and Changing Environment Variables in Windows 10

Setting the path and variables in Windows 10
1. From the desktop, right-click the very bottom-left corner of the screen to get the Power User Task Menu.
2. From the Power User Task Menu, click System.
3. In the Settings window, scroll down to the Related settings section and click the System info link.
4. In the System window, click the Advanced system settings link in the left navigation pane.
5. In the System Properties window, click on the Advanced tab, then click the Environment Variables button near the bottom of that tab.
6. In the Environment Variables window (pictured below), highlight the Path variable in the System variables section and click the Edit button. Add or modify the path lines with the paths you want the computer to access. Each different directory is separated with a semicolon, as shown below.

    C:\Program Files;C:\Winnt;C:\Winnt\System32
    
![Setting Environmental Variables in Windows 10](https://www.screencast.com/t/fUE7yOVni1)

*Note*
You can edit other environment variables by highlighting the variable in the System variables section and clicking Edit. If you need to create a new environment variable, click New and enter the variable name and variable value.

#### Use the Salesforce CLI from Behind a Company Firewall or Web Proxy
If you install or update the Salesforce CLI on a computer that’s behind a company firewall or web proxy, you sometimes receive error messages. In this case, you must further configure your system.
You get an error similar to the following when you run a command after installing the CLI binary behind a firewall or web proxy. This error is from a Linux computer, but Windows and macOS users sometimes see a similar error.

    sfdx-cli: Updating CLI... !

     ▸  'ECONNRESET': tunneling socket could not be established, cause=connect EHOSTUNREACH 0.0.23.221:8080 - Local (10.126.148.39:53107)

To address this issue, run these commands from your terminal or Windows command prompt, replacing username:pwd with your web proxy username and password. If your proxy doesn’t require these values, omit them. Also replace proxy.company.com:8080 with the URL and port of your company proxy.

    npm config set https-proxy https://username:pwd@proxy.company.com:8080
    npm config set proxy https://username:pwd@proxy.company.com:8080

Then set the HTTP_PROXY or HTTPS_PROXY environment variable to the full URL of the proxy. On a Windows machine:

    set HTTP_PROXY=https://username:pwd@proxy.company.com:8080
    set HTTPS_PROXY=https://username:pwd@proxy.company.com:8080

    export HTTP_PROXY=https://username:pwd@proxy.company.com:8080
    export HTTPS_PROXY=https://username:pwd@proxy.company.com:8080


If You Still See an Error
Your Proxy Requires an Extra Certificate Authority

If you set the proxy environment variable, and you still see error messages, it’s possible that your proxy requires an extra certificate authority (CA). Ask your IT department where to find or download the certificates.

Set this environment variable to point to the CA file: NODE_EXTRA_CA_CERTS.

Your Corporate Network Is Blocking Salesforce Hosts

It’s possible that your corporate network is blocking the Salesforce hosts for updating or installing Salesforce CLI. Contact your IT department to whitelist these domains:

    https://developer.salesforce.com/media/salesforce-cli
    https://registry.npmjs.org

