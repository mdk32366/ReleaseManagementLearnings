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

5. Optionally, from the ANT_HOME directory run ant -f fetch.xml -Ddest=system to get the library dependencies of most of the Ant tasks that require them. If you don't do this, many of the dependent Ant tasks will not be available. See Optional Tasks for details and other options for the -Ddest parameter.

6. Optionally, add any desired Antlibs. See Ant Libraries for a list.

Note that the links in the list above will give more details about each of the steps, should you need them. Or you can just continue reading the rest of this document.
