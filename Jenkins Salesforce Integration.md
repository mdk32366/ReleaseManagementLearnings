### Getting Jenkins Up and Running with Salesforce

[ANT (Force) Migration Tool](https://developer.salesforce.com/docs/atlas.en-us.daas.meta/daas/meta_development.htm)
[Jenkins](https://wiki.jenkins.io/display/JENKINS/Meet+Jenkins)

#### Step 1: Download the Force.com Migration Tool
Ha – bet you thought the first step would be download Jenkins.  Well, close – but there’s no point in getting up and running with Jenkins without getting your feet wet with the Force.com Migration Tool.  While developers often become familiar with the Eclipse-based tools, the Force.com Migration Tool sometimes becomes a rarer treat.  It is, however, an extremely versatile tool that allows for easy command-line deployment.  The Force.com Migration Tool is an Apache Ant based solution, which is what makes it compatible with other tools like Jenkins so easily.

So jump over to the Force.com Migration Tool’s wiki page, get acquainted with it, install it – and we’ll see you at Step 2.

#### Step 2: Meet Jenkins
Head over to the Jenkins welcome page – which serves as a quick introduction and also offers some different installation paths.  Jenkins even offers a download free test drive on that page, which is pretty cool.  Pick the installation path that makes sense for you.  For the record, I’ve got it downloaded locally on OSX, and I use the following bash script to fire it off:

    #!/bin/bash
    nohup java -jar ~/Projects/jenkins.war --httpPort=8880 > ~/jenkins.log 2>&1 &

Once running, you should see a screen like this:

![Jenkins App Main](http://res.cloudinary.com/hzxejch6p/image/upload/v1371104390/ofdem0wmgy3mv8woambi.png)

#### Step 3: Configure Jenkins
Now we need to create a fresh job in Jenkins. Click “New Job”, and then select the first radio button for the free-style project:

![Demo Project](http://res.cloudinary.com/hzxejch6p/image/upload/v1371104391/zh5ffsuf7cs0mxzgmkgt.png)

The OK button should appear, click that to create the job.  It will now appear on the Jenkins dashboard.  Click the new job in the dashboard, and then go to “Configure” in the left nav.

Notice the Source Code Management section at the top of the configuration page.  This is where you will setup the link to your source control.  Since everyone’s source control is different, you’ll need to put in your specific information here.  For instance, I installed the Git and Github plugins and just use my generic developer repo from there.  You can then select your build triggers.  Probably the easiest default selection is have it build on a cycle, via the Build Periodically checkbox.  You can setup a cron-style schedule from there.  There are more advanced features, like building whenever a Github repo is updated – but that requires an external URL that Github can find.

To tie Jenkins to the Force.com Migration Tool, you need to go to the Build Panel and select some Invoke Ant tasks.  One of mine looks like this:

![Tying in ANT(FORCE) Migration Tool

And that is pointing and Ant friendly build file, which will resemble something like:

    <project default="test" basedir="." xmlns:sf="antlib:com.salesforce">

        <property file="build-ji.properties"/>
        <property environment="env"/>
    
        <target name="test">
          <sf:deploy username="${username}" password="${password}" serverurl="${serverurl}" deployRoot="./" runAllTests="true" />
        </target>
        <target name="clean">
          <sf:deploy username="${username}" password="${password}" serverurl="${serverurl}" deployRoot="clean" />
      
        </target>
    
    </project>

And a properties file, which looks like this:

    username=username@something.com
    password=password
    serverurl=https://login.salesforce.com

Obviously with your credentials. You could potentially hardcode some values as well, how I have it above isn’t the only way to do it.

Once you have that configured, click Save.   Now you can kick off a new build via the Build Now link in the left nav.  Jenkins will run through everything in the configuration and monitor the output.  You’ll see the progress bar get kicked off, and then once everything is done running – you can check the results via the Console Output from the specific build record.  If I wanted an automatic notification of the success or fail, I can easily add that as a Post Build step in the configuration.

#### Just the Beginning
That’s really only scratching the surface of leveraging Jenkins and Ant.  For example, take a look at Kevin O’Hara‘s Grunt.js based build script, which manages all the optimizations they need on files before translating them into static resources for deployment.  Also check out Eric Wilcox’s excellent Dreamforce session on Team Development, and the companion tools he created.  You might also look at Kevin Bromer‘s article on setting up Jenkins for Force.com with AWS.

In upcoming blog posts, I’ll share some of the Bash scripts I use to maintain local projects as well.  As usual, if you’ve got comments or questions – leave them in the boxes below or catch me on twitter @joshbirk.

#### Jenkinsfile Walkthrough
The sample Jenkinsfile shows how to integrate Salesforce DX into a Jenkins job. The sample uses Jenkins multibranch pipelines. Every Jenkins setup is different. This walkthrough describes one of the ways to automate testing of your Salesforce applications. The walkthrough highlights the Salesforce DX CLI commands to create a scratch org, upload your code, and run your tests.
We assume that you are familiar with the structure of the Jenkinsfile, Jenkins Pipeline DSL, and the Groovy programming language. This walkthrough focuses solely on Salesforce DX information. See the Salesforce DX Command Reference regarding the commands used.

This Salesforce DX workflow most closely corresponds to Jenkinsfile stages.

#### Define Variables
Use the def keyword to define the variables required by the Salesforce DX CLI commands. Assign each variable the corresponding environment variable that you previously set in your Jenkins environment.

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

Define the SFDC_USERNAME variable, but don’t set its value. You do that later.

    def SFDC_USERNAME

Although not required, we assume you’ve used the Jenkins Global Tool Configuration to create the toolbelt custom tool that points to the CLI installation directory. In your Jenkinsfile, use the tool command to set the value of the toolbelt variable to this custom tool.

    def toolbelt = tool 'toolbelt'
