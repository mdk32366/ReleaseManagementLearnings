### [ANT Migration Tool](https://developer.salesforce.com/docs/atlas.en-us.daas.meta/daas/meta_development.htm)

To get up and running with the binary distribution of Ant quickly, follow these steps:

1. Make sure you have a Java environment installed. See System Requirements for details.

2. Download Ant. See [Binary Distribution](https://ant.apache.org/bindownload.cg) for the ZIP version used to install to Windows.

3. Uncompress the downloaded file into a directory. (Recommend C:/ANT)

4. Set environmental variables: JAVA_HOME to your Java environment, ANT_HOME to the directory you uncompressed Ant to, and add ${ANT_HOME}/bin (Unix) or %ANT_HOME%/bin (Windows) to your PATH. See Setup for details.

You can check the basic installation with opening a new shell and typing ant. You should get a message like this

Buildfile: build.xml does not exist!
Build failed
So Ant works. This message is there because you need to write a buildfile for your project. With a ant -version you should get an output like

Apache Ant(TM) version 1.9.2 compiled on July 8 2013
If this does not work, ensure your environment variables are set right. E.g., on Windows, they must resolve to:

See: [Setting Environmental Variables in Windows 10](https://github.com/mdk32366/ReleaseManagementLearnings/blob/master/Dev%20Hub%20and%20Salesforce%20DX%20Setup.md#setting-and-changing-environment-variables-in-windows-10)

required: 
    
    %ANT_HOME%\bin\ant.bat

optional: 
    
    %JAVA_HOME%\bin\java.exe

required: 
    
    %PATH%=...maybe-other-entries...;%ANT_HOME%\bin;...maybe-other-entries...

ANT_HOME is used by the launcher script for finding the libraries. JAVA_HOME is used by the launcher for finding the JDK/JRE to use. (JDK is recommended as some tasks require the Java tools.) If not set, the launcher tries to find one via the %PATH% environment variable. PATH is set for user convenience. With that set you can just start ant instead of always typing the/complete/path/to/your/ant/installation/bin/ant.

The CLASSPATH environment variable is a source of many Ant support queries. As the round trip time for diagnosis on the Ant user mailing list can be slow, and because filing bug reports complaining about 'ant.bat' not working will be rejected by the developers as WORKSFORME "this is a configuration problem, not a bug", you can save yourself a lot of time and frustration by following some simple steps.

Do not ever set CLASSPATH. Ant does not need it, it only causes confusion and breaks things.
If you ignore the previous rule, do not ever, ever, put quotes in the CLASSPATH, even if there is a space in a directory. This will break Ant, and it is not needed.
If you ignore the first rule, do not ever, ever, have a trailing backslash in a CLASSPATH, as it breaks Ant's ability to quote the string. Again, this is not needed for the correct operation of the CLASSPATH environment variable, even if a DOS directory is to be added to the path.
You can stop Ant using the CLASSPATH environment variable by setting the -noclasspath option on the command line. This is an easy way to test for classpath-related problems.
The usual symptom of CLASSPATH problems is that Ant will not run with some error about not being able to find org.apache.tools.ant.launch.Launcher, or, if you have got the quotes/backslashes wrong, some very weird Java startup error. To see if this is the case, run ant -noclasspath or unset the CLASSPATH environment variable.

You can also make your Ant script reject this environment variable just by placing the following at the top of the script (or in an init target):

    <property environment="env."/>
    <property name="env.CLASSPATH" value=""/>
    <fail message="Unset $CLASSPATH / %CLASSPATH% before running Ant!">
        <condition>
            <not>
                <equals arg1="${env.CLASSPATH}" arg2=""/>
            </not>
        </condition>
    </fail>

5. Optionally, from the ANT_HOME directory run 

    ant -f fetch.xml -Ddest=system 

to get the library dependencies of most of the Ant tasks that require them. If you don't do this, many of the dependent Ant tasks will not be available. See Optional Tasks for details and other options for the -Ddest parameter.

6. Optionally, add any desired Antlibs. See Ant Libraries for a list.

Ant supports a number of optional tasks. An optional task is a task which typically requires an external library to function. The optional tasks are packaged together with the core Ant tasks.

* The external libraries required by each of the optional tasks is detailed in the Library Dependencies section. These external libraries must be added to Ant's classpath, in any of the following ways:

* In ANT_HOME/lib. This makes the JAR files available to all Ant users and builds.

* In ${user.home}/.ant/lib (since Ant 1.6). This allows different users to add new libraries to Ant. All JAR files added to this directory are available to command-line Ant.

* On the command line with a -lib parameter. This lets you add new JAR files on a case-by-case basis.

**In the CLASSPATH environment variable. Avoid this; it makes the JAR files visible to all Java applications, and causes no end of support calls. See below for details.**

In some <classpath> accepted by the task itself. Since Ant 1.7.0, you can run the <junit> task without junit.jar in Ant's own classpath, so long as it is included (along with your program and tests) in the classpath passed when running the task.

Where possible, this option is generally to be preferred, as the Ant script itself can determine the best path to load the library from: via relative path from the basedir (if you keep the library under version control with your project), according to Ant properties, environment variables, Ivy downloads, whatever you like.

If you are using the binary distribution of Ant, or if you are working from source code, you can easily gather most of the dependencies and install them for use with your Ant tasks. In your ANT_HOME directory you should see a file called fetch.xml. This is an Ant script that you can run to install almost all the dependencies that the optional Ant tasks need.

To do so, change to the ANT_HOME directory and execute the command:

    ant -f fetch.xml -Ddest=[option]

where option is one of the following, as described above:

_system—store in Ant's lib directory (Recommended)_

_user—store in the user's home directory_

_optional—store in Ant's source code lib/optional directory, used when building Ant source code_

Note that the links in the list above will give more details about each of the steps, should you need them. Or you can just continue reading the rest of this document.
