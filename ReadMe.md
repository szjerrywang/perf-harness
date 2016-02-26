# Perfharness
Perfharness is a flexible and modular Java package for performance testing of HTTP, JMS, MQ and other transport scenarios. It provides a complete set of transport functionality, as well as many other features such as throttled operation (a fixed rate and/or number of messages), multiple destinations, live performance reporting, JNDI. It is one of the many tools used by performance teams for IBM MQ and IBM Integration Bus in order to conduct tests ranging from a single client to more than 10,000 clients. Customers also use the tool to test their own systems and validate they perform accordingly.

Perfharness has been available in binary form on [IBM AlphaWorks](https://ibm.biz/JMSPerfHarness) for a number of years. 

# Importing Perfharness into eclipse.

The top level folders in the repository are eclipse projects. 
* Download the repository zip file and extract into a temporary directory.  
* Import the projects using the Import Wizard selecting "Existing Projects into Workspace". 
* Select the 5 projects and make sure "Copy projects to workspace" is ticked when importing.

This should result in you having a workspace with the following projects:

![Workspace](images/PerfharnessWorkspace.png?raw=true "Workspace")

## Importing Prereqs into eclipse

Building Perfharness in eclipse requires some extra dependancies importing to the workspace. Depending on the modules you would like to build please import the following files into the PerfHarnessPrereqs project, these files are either available on the web from Apache or from the respective product installations. JMSPerfharness requires WebSphere MQ8, MQJavaPerfHarness requires WebSphere MQ7 (or above) and the Perfharness and HTTPPerfharness modules have no prereqs

* IBM_WMQ_7: 
    * com.ibm.mq.commonservices.jar
    * com.ibm.mq.headers.jar
    * com.ibm.mq.jar
    * com.ibm.mq.jmqi.jar
    * com.ibm.mq.pcf.jar
    * com.ibm.mqjms.jar
    * com.ibm.msg.client.jms.internal.jar
    * com.ibm.msg.client.jms.jar
    * connector.jar
    * fscontext.jar
    * jms.jar
    * jndi.jar
    * jta.jar
    * ldap.jar
    * providerutil.jar
* IBM_WMQ_8: 
    * com.ibm.mq.headers.jar
    * com.ibm.mq.jar
    * com.ibm.mq.jqmi.jar
    * com.ibm.mqjms.jar
    *  com.ibm.mq.pcf.jar
    * jms.jar
* ant-contrib.jar (Always required)

## Running Perfharness within Eclipse

Once you have imported the pre-reqs you are ready to run within Eclipse or compile the jar for using on other machines. If you would like to run perfharness create a new Run Configuration for a Java application. Generally we would recommend running from eclipse for functional and setup testing but for real performance testing using the jar from the command-line on a server.

Select "JMSPerfHarnessMain" as the Project and "JMSPerfHarness" as the Main Class as shown below

![Run Configuration Class](images/PerfharnessRunConfiguration1.png?raw=true "RunConfigurationClass")

On the "Arguments Tab" insert the appropriate set of program arguments such as (MQ Receiver Example):

![Run Configuration Arguments](images/PerfharnessRunConfiguration2.png?raw=true "RunConfigurationArguments")

Click "Run"


## Compiling perfharness.jar within Eclipse

There are 4 scripts which can be used to compile Perfharness.jar depending on whether you want all the capability or a specific one. The 4 scripts are:

* Perfharness/build_all.xml - Used for building the whole package.
* Perfharness/build_HTTP&TCPIP.xml - Used for building only the http & tcpip modules.
* Perfharness/build_JMS.xml - Used for building the JMSMQ module.
* Perfharness/build_MQ.xml - Used for building the MQ module.

To compile perfharness.jar right click the the appropriate .xml file and select "Run As" then "1 Ant Build". 

![Run As](images/RunAs.png?raw=true "RunAs")

This should launch the build process which will produce the jar file perfharness.jar

![JarBuilt](images/PerfHarnessBuilt.png?raw=true "JarsBuilt")

### Sample Commands:

MQ Requestor:

```java -ms512M -mx512M -cp /PerfHarness/perfharness.jar JMSPerfHarness -tc mqjava.Requestor -nt 1 -ss 5 -sc BasicStats -wi 10 -to 3000 -rl 60 -sh false -ws 1 -dn 1 -mf <yourInputFile> -jh localhost -jp 1414 -jb CSIM  -jc SYSTEM.BKR.CONFIG -iq CSIM_SERVER_IN_Q -oq CSIM_COMMON_REPLY_Q```

HTTP Requestor:

```java -ms512M -mx512M -cp /PerfHarness/perfharness.jar JMSPerfHarness -tc http.HTTPRequestor -nt 1 -ss 5 -sc BasicStats -wi 10 -to 3000 -rl 60 -sh false -ws 1 -dn 1 -mf MyInputMessage.xml -jh localhost -jp 7800 -ur "requestinout"```

HTTPS Requestor:

```java -ms512M -mx512M -Djavax.net.ssl.trustStore=httpsCAPHTrustStore.jks -Djavax.net.ssl.trustStorePassword=MyPassw0rd -Djavax.net.ssl.trustStoreType=JKS -cp /PerfHarness/perfharness.jar JMSPerfHarness -tc http.HTTPRequestor -nt 1 -ss 5 -sc BasicStats -wi 10 -to 3000 -rl 60 -sh false -ws 1 -dn 1 -mf MyInputMessage.xml -jh localhost -jp 7800 -ur "requestinout" -se true```

SOAP Requestor:

```java -ms512M -mx512M -cp /PerfHarness/perfharness.jar JMSPerfHarness -tc http.HTTPRequestor -nt 1 -ss 5 -sc BasicStats -wi 10 -to 3000 -rl 60 -sh false -ws 1 -dn 1 -mf <yourInputFile> -jh localhost -jp 7800 -ur "SoapProvider" -sa SummerSale```

TCPIP Requestor: 

```java -ms512M -mx512M -cp /PerfHarness/perfharness.jar JMSPerfHarness -tc tcpip.TCPIPRequestor -nt 1 -ss 5 -sc BasicStats -wi 10 -to 3000 -rl 60 -sh false -ws 1 -dn 1 -mf MyInputMessage.xml -jh localhost -jp 1455 -rb 1384```

### Common Flags:

```-tc transport
-iq Input Queue
-oq Output Queue
-jp MQ Listener Port
-jb Queue Manager Name
-jc Queue Manager Channel name
-jh Endpoint Hostname
-rb Receive Buffer
-to Request Timeout
-ss Snapshot interval
-sc Statistics Module
-mg Input Sample Data filename
-nt Number of Threads
-sh Intercept crtl-c
-rl Run Length
-wi wait interval in ms between starting clients
```
For more in-depth documentation please refer to the PerfHarness manual in the PerfHarness/docs/ folder.