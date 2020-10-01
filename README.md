---

# Joint Longitudinal Viewer (JLV)

JLV is a centrally hosted, Java-based web application managed as a single code
baseline and deployed in separate DoD (Department of Defense) and VA (Department of Veterans Affairs) environments.
Its browser-base, graphical user interface (GUI) provides an integrated, read-only view of
Electronic Health Record (EHR) data from the VA, DoD, and community partners, within in a single
application.

JLV eliminates the need for VA and DoD clinicians to access disparate viewers. The GUI retrieves clinical data 
from several native data sources and systems, then presents it to the user via widgets, each corresponding to a clinical data domain. 
Users can create and personalize tabs, drag and drop widgets onto tabs, sort data within a widget’s columns, set date filters, and expand a widget 
for a detailed view of patient information.

---

## Table of Contents


* <a href="#product-information">Product Information</a>
* <a href="#jlv-description">JLV Description</a>
* <a href="#configurations">Configurations</a>
* <a href="#launching-applications">Launching Applications</a>
* <a href="#build">Building</a>



## Product Information

- Product Name: Joint Longitudinal Viewer (JLV)
- Product Line Portfolio: HI&M (Health Integration and Modernization)

### VA OIT Management Team Contacts


| Role                   |              Description        |
| ----------- | ----------- |
| JLV Project Manager      | Colleen Reed              |
| CV Project Manager        | Vincent Freeny        |
| JLV and CV Testing Lead | Jerilyn Roberts |
| JLV and CV Functional Analyst | Bill Beloro |
| CV Functional Analyst | Michelle Boie |
| 

### Liberty IT Solutions Team Contacts


| Role                   |              Description |
| ----------- | ----------- |
| Project Manager       | Lori Thomas               |
| Project Owner         | Michelle Alexander        |
| Configuration Manager | Jo Ann Nelson             |
| 


- Maintained by: @department-of-veterans-affairs/configuration-management




## JLV Description

JLV is a Java-based, web application that utilizes Grails MVC with groovy syntax, JavaScript, 
HTML, CSS, jQuery, and Ajax to achieve the GUI interface. REST and SOAP calls are made between jMeadows, which
is a web service that aggregates patient and provider data for clinical domains.

### Abstraction/Application Tier

---

#### jMeadows

jMeadows is a web service that aggregates patient and provider data for clinical domains. It is an essential component of the JLV GUI framework that uses Java-based 
web service technology and request- and response-driven transactions for its web service system interfaces. JLV utilizes the Defense Manpower Data Center’s (DMDC’s) PDWS and the MVI for patient searches. jMeadows sends search requests and retrieves 
and aggregates patient data. jMeadows uses SOAP version 1.2 messaging protocol to communicate with PDWS, which contains a list of all federal employees and provides their Enterprise Federal Identification (ID) for patient lookup; the VA MVI Enterprise Central Web Service, which provides the VA enterprise 
unique patient identifier information; and data source interfaces, such as VDS and the Relay Service, which are used to call each EHR system in which a patient is registered.

#### Report Builder

The Report Builder tool is used to create and print reports that include multiple patient records from supported clinical data domains. Report Builder works asynchronously, 
allowing users to continue using other JLV features while a report is processing in the background.  The interface between the jMeadows Data Service and the Report Builder Service is a software interface, utilizing SOAP 
version 1.2 as the messaging protocol that communicates between the systems. The interface between the JLV web application and the Report Builder Service is a software interface, utilizing REST as the messaging protocol that communicates 
between the systems. The system transmits data/records between the JLV DB to the Report Builder Service utilizing JDBC. 

#### Data/Storage Tier

---

#### Vista Data Service

VDS is a web service that retrieves VA-specific clinical data from all VistA host systems where a patient is registered, primary care team demographics data 
from Patient Centered Management Module (PCMM) web service and images from CVIX and MUSE. VDS uses RPCs to 
pull clinical information and retrieve data from a VistA host system. VDS also interfaces with jMeadows for VA user 
authentication from local VistA host systems. 

#### Relay Service

The Relay Service is a web service that retrieves DOD-specific clinical data from Data Exchange Service (DES). 
DES interfaces with the following DOD services:
* Cerner Millennium (FEHR) (formerly MHS GENESIS)
* DAS
* HAIMS
* Joint HIE 
(Community Partner Data)
* Armed Forces Health Longitudinal Technology Application (AHLTA) CDR, DOD Share(s), TMDS, and the 
BHIE Framework.

#### EHRM Service

The EHRM Service is a web service within the data/storage tier of JLV, where source application’s data is stored and from where data is retrieved. 
To retrieve data, information requests are passed from jMeadows interface and through the EHRM Service and FHIR API to Cerner Millennium FEHR. 

The EHRM Service is also used as a backup to connect to Joint HIE when DES is unavailable. EHRM Service retrieves Community Partner data (through Joint HIE), 
information requests are passed from jMeadows interface and through the EHRM Service. Data is then returned through the EHRM Service and back to jMeadows to 
be displayed in the JLV Community Health Summaries and Documents widget.

#### DB Information
The JLV DB is a Microsoft (MS) Structured Query Language (SQL) Server 2012 DB that stores user profile information and audit records. The DB also stores local 
and national standard terminology mappings for VA and DOD. It does not store patient or provider data from VA and DOD EHR systems, neither long-term nor temporarily.
The JLV DB resides on a dedicated server within a deployed JLV environment, alongside the server hosting the JLV web application and VDS. The JLV web application 
and components of the JLV system, including jMeadows, are the only components to connect and utilize the JLV DB.

## Configurations 

#### Configs for Local Development on your GFE


#### Things You'll Need: 

* JDK 1.7
* JDK 1.8
* Grails 2.4.4
* Apache Maven 3.6.3
* Apache Ant 1.9.15
* IntelliJ Idea
* Git Client - Confirm it is available on the [TRM](https://www.oit.va.gov/Services/TRM/ToolListSummaryPage.aspx?lob=1) prior to installation.

These can be found on the [Liberty JLV Sharepoint Site](https://libertyits.sharepoint.com/:u:/r/sites/HIM_JLV/Shared%20Documents/Software%20Engineering/Requirements/Software/java.zip?csf=1&web=1&e=iUuhVd) or downloaded directly from their respective websites.

#### Setting Up Your Local Environment

Unzip the file containing all of the configurations to your local C:\ drive - you'll end up with a path **C:\Java**. 

Ensure you have IntelliJ Idea installed, as well as a git client of your choice.

##### Environment Variables

Next, open the **Account Environment Variables** system configuration page by clicking the start menu and typing *Variables* - ensure you've selected the option labeled **Edit Environment Variables For Your Account**, as there will be two options and the other one requires administrative rights. 

The following environment variables will need to be added: 

| Variable | Value | 
| --- | --- | 
| ANT_HOME | C:\java\apache-ant-1.9.15 | 
| GRAILS_HOME | C:\java\grails-2.4.4 | 
| JAVA_HOME | C:\java\jdk1.7.0_80 | 
| MAVEN_HOME | C:\java\apache-maven-3.6.3 | 

Next, the following entries will need to be added to your **Account** Path (This is found in the Environment Variables as well, labeled *PATH*): 

`%JAVA_HOME%\bin`

`%GRAILS_HOME%\bin`

`%GRAILS_HOME%`

`%MAVEN_HOME%\bin`

`%ANT_HOME%\bin`


##### Configuration Files

Each application node has an appconfig-_environment_.properties configuration file located in the following locations: 

| Service | Location |
| --- | --- |
| ehrmservice | ehrmservice\src\main\resources\appconfig-_environment_.properties | 
| HuiCore | `unsure if this is a thing` | 
| JLV | JLV\grails-app\conf\spring\appconfig-_environment_.properties | 
| JLVQoS | JLVQoS\src\main\resources\appconfig-_environment_.properties | 
| jMeadows | jMeadows\src\main\resources\appconfig-_environment_.properties | 
| reportbuilder | reportbuilder\src\main\resources\appconfig-_environment_.properties | 
| VistaDataService | VistaDataService\src\main\resources\appconfig-_environment_.properties | 

###### Configuration Versions

The correct application version file must be copied to the above location and named for the environment which will be launched. Known application versions are: 

* DTE-Silver

* DTE-Gold

* Production

## Launching Applications

### Launching the JLV application

In IntelliJ, browse to the JLV application location. After copying the configurations, open at terminal and type the below: 

`grails dev run-app` 


### Launching other applications

Unfortunately, we don't know this yet. :hurtrealbad:

## Testing

## Build

#### Maven Build

##### jMeadows | ReportBuilder | JLVQoS | VistaDataService | EHRMService

To build, you will need to download and unpack 3.6.3 version of Maven (https://maven.apache.org/download.cgi) and put the `mvn` command on your path. Then, you will need to install a Java 1.7 (or 1.8 depending on project) JDK (not JRE!), 
and make sure you can run `java` from the command line. Now you can run `mvn clean install` and Maven will compile your project, and put the results it in a jar file in the target directory.
Maven uses the supplied 'pom.xml' file which describes
exactly how to build your project.

To build project, navigate to project directory:

Environment options: development, production

The default environment is development if one is not specified.

| Project        | Java Version           | Build Command  |
| ------------- |:-------------:| :-----:|
| ehrmservice      | 8 | `mvn -e clean install  -Dhttps.protocols=TLSv1.2  -Pproduction`  |
| JLV      | 7      |  `grails dev run-app --stacktrace -Dhttps.protocols=TLSv1.2`   |
| JLVQoS | 7      |    `mvn -e clean install  -Dhttps.protocols=TLSv1.2  -Pproduction`  |
| jMeadows | 7      |   `mvn -e clean install  -Dhttps.protocols=TLSv1.2  -Pproduction`   |
| reportbuilder | 7      |    `mvn -e clean install  -Dhttps.protocols=TLSv1.2  -Pproduction`  |
| VistaDataService | 7      |    `mvn -e clean install  -Dhttps.protocols=TLSv1.2  -Pproduction` |


#### Ant Build

Download Ant 1.9.15

https://ant.apache.org/bindownload.cgi

Navigate to `./HuiCore` and

Run `ant all` command in order to clean/build/package project.

| Project        | Java Version           | Build Command  |
| ------------- |:-------------:| :-----:|
| HuiCore      | 7 | `ant all`  |

Run the additional following commands if needed in the project directory:

* `ant -p` to list all available targets,
* `ant all` build all
* `ant artifact.huicore:jar` Build 'HuiCore:jar' artifact
* `ant build.all.artifacts` Build all artifacts
* `ant build.modules` build all modules
* `ant clean` to clean up project folder.
* `clean.module.huicore`               cleanup module
* `compile.module.huicore`             Compile module HuiCore
* `compile.module.huicore.production`  Compile module HuiCore; production classes
* `compile.module.huicore.tests`       compile module HuiCore; test classes
* `init`                             Build initialization


#### Branching strategy

**UPDATE THIS FOR YOUR PROJECT AND BUILD STRATEGY**

Certain branches are special, and would ordinarily be deployed to various test environments:

- master: our default branch, for production-ready code. Master is always deployable. In our case, however, deployment does not happen automatically.
- pre-prod: code destined for the pre-production test server. This code might be deployed by hand or automatically, depending on the project and availability of a CI/CD solution.
- test: code that would probably autotmatically be pushed to a test or staging server. Again, in our case we don't do this -- but test deployment tasks like this are ideally automated with a CI/CD solution like Jenkins.

New code should be produced on a feature branch [following GitHub flow](https://guides.github.com/introduction/flow/). Most often, you'll want to branch from **master**, since that's the latest in production. File a pull request to merge into **test**, which can be deployed to our testing environment.




### License

See the [LICENSE](LICENSE.md) file for license rights and limitations.
