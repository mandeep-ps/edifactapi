# Edifact D09A API #

## Quick start ##
EdifactAPI project convert Edifact D09A standard's messages (invoice, order etc.) to excel definition files and schema files that can be used e.g. for creating Web Services. Below are **invoice message** examples of these files and web service:

  1. [Invoice definition excel](http://www.vnetcon.org/edifactdocs/beta-0.5-INVOIC_D09A/INVOIC_D09A.xlsx) (For some reason my IE tries to handle this as zip file?!?! Mozilla and Chrome works fine)
  1. [Invoice schema](http://www.vnetcon.org/edifactdocs/beta-0.5-INVOIC_D09A/schema1.xsd)
  1. [Invoice Web Service wsdl](http://www.vnetcon.org/invoice?wsdl) Invoice Web Service wsdl generated on line by wss running on Google App Engine (Chrome shows only blank page)
  1. [Invoice Web Service xsd](http://www.vnetcon.org/invoice?xsd=1) Invoice Web Service xsd generated on line by wss running on Google App Engine (Chrome shows only blank page)

Excel is for defining message requirements, schema is for developers and web service is for partners to send messages using Web Services.

If you have any problems viewing files at 1 and 2 please download zip file from download section. Each zip contains both excel and schema file for message.

## What is edifact D09A? ##

In short edifact is a standard which contains about 200 messages organizations **can** use to exchange data. (The can is bold because it is good to know that it is **not a must** to implement all messages. You might want to implement only e.g. invoice message) Edifact D09A is one version of this standard.

Edifact is a global standard used in EDI. EDI itself is used by big companies in Electronic Data Interchange. In other words exchanging data between organizations.

You can find out more information and original specifications of edifact here:

http://www.unece.org/trade/untdid/welcome.htm

The specifications are located at Directories -> Download Directories - ZIP files



## What is this project? ##

This projet's goal is to make EDI implementation easier, cheaper and faster so that small companies and organizations can also benifit of EDI. Our focus in this project is more on information (where it is deliverd and what parties has agreed about information) than on "syntax of EDI file". In this project "the file syntax" is implemented as xml files.

This philosophy you can see in our implementation of EDI: Schemafile + Excel definition file. In these files we are more "project orientated" than software orientated. This project don't offer any kind of "edi file parsers" or "mapping tools" for transforming files from one format to an other. This project goal is to use one format: Xml files that are based on edifact standard. (We do have plans to implement edifact file reader/writer for transforming edifact files into xml files and vise versa but this will be more "a helper util" when moving from current system to xml based edi.)

### Excel definition files ###
These files are the cornerstones for whole edifactapi project. There is one excel file for each EDI message where all the fields that are possible to use in this message are listed. The actual job need to be done using these files is to find right fields what to use in messaging so that both the sender and receiver knows what information is delivered in what fields. (There are really a lot of fields and you will probably use only a really small subset of these fields)

Filling these excels supports directly developers and make it much easier for them to implement the actual messages.


### Schema files ###
Schema files are ment for developers for implementing the definitions from excel files. Schema files are "replacements" for "edifact files" in this solution. The naming between schema files and excel files is identical so the developers can easily set and get the information to and from xml based messages.

From schema files developers can easily create required classes they need in development.
E.g. with javatools you need only run xjc and you done. Below is an example command for doing this.

c:>myedifactproject\schemas\**xjc -p org.mycompany.edifact.schemas.invoic\_d09a schema1.xsd**

From xjc generated files developers can quite easily develop Web Services that can be used for receiveing messages from partners. After the Web Service is up and running partners can easily create classes for accessing the Web Service. Below is an example command for doing this with javatools.

c:>myedifactclientproject\parties\companya\**wsimport -keep http://url.to.webservice/invoic_d09a?wsdl***

Web Service based architectre is known today as SOA (Service Orientated Architectre) and in general there is good support for this in several development tools and IDE (E.g. Ecliplse, NetBeans, Visual Studio, command line java tools etc.)

If you want to see real WebService in action running on Google App Engine, please click the following link:

http://www.vnetcon.org/invoice?wsdl


This will show you EdifactAPI project's invoice spcification part served as WebService, powered by WebSerivceServlet.




## Downloads ##
At downloads you can download both excel and shema files in one zip. There are about 200 zip files and each file represent one message (e.g. invoice, order) in edifact specification.

At the moment only a subset of messages can be download due to space "challenges".
We have apply more space and the rest of messages will be added later.

## How are these files done? ##
We have developed a program that parses all the information from native edifact d09a specifications. The generation process is following:

  1. First the program will generate java files from common elements
  1. Second the program will generate java files from each message
  1. During message generation the program creates also an excel file related to message
  1. During message generation the program writes command file which contans commands to run schemagen (java tool for creating schema files from java files) and jar commands for packing schema and excel files into zip
  1. after program exceqution the command file is run and the zip files are ready for upload

## Current solutions versus EdifactAPI solutions ##
Below is a picture where you can see the diffrencies between current oparator based solutions versus EdifactAPI solution. Please note that this pictre does not cover the whole operator world or eghter EdifactAPI solution - it is only a simple high level picutre to get some understanding of differencies.


![http://docs.vnetcon.org/images/home/edifactapi_bigpicture.png](http://docs.vnetcon.org/images/home/edifactapi_bigpicture.png)



From this picture it is possible to see why integration (edi and others) are so difficult today:
  1. operators support any file types from senders and receivers
  1. files are transformed several times and with complex files it take time before every transform in the chain is correct
  1. Using EdifactAPI you will need only one "Message Content Agreement" between sender and receiver. In operator world you need several "mappings agreements" between user files and inhouse standards.
  1. Quite often the import-/export files have to be write from scratch by developers

Extra bonus using EdifactAPI is that you can send and receive messages directly without operators. And if you are using Google App Engine you don't have to buy or rent servers where you can run your messaging Web Services. If you have a really a lot of messages then you might have to pay something to get more resources, otherwise using Google App Engine is free.

Unfortunately Google App Engine don't currently support Web Services. Because of this we have started a subproject to overcome this issue. This project can be found at http://code.google.com/p/webserviceservlet/


## What are EdifactAPI project steps? ##
### Offering message interface for partners (Setting up Web Services) ###
These are the required steps to be done if you start from scratch:

  1. Fill excel definition file for messages you want to receive
  1. Generate EdifactAPI classes using e.g. xjc (java tool)
  1. Create Web Services for messages
  1. Integrate Web Services to you legacy system

### Accessing partners message interface (Calling Web Serivces) ###

  1. Read the service offerer's excel definition file
  1. Create client side classes for service using e.g. wsimport (java tool)
  1. Integrate Web Service classes to legacy system


## About security ##
Basically you use the Web Service platforms security (https, authentication etc.) to secure you messages. Because it is also possible to use EdifactAPI in traditional way like file transfer using (s)ftp or even Google Shared folders then this kind of "Web Service" security is not possible. To overcome this we have started a project called pkimessages which uses standard pki methods for securing messages deliverd e.g. using shared folders, email, ftp etc.

You can find this project at http://code.google.com/p/pkimessage/

## Why Edifact and not e.g. UBL? ##
This is a good question. We end up with edifact for following reasons:

  * edifact is really widely used today
  * there are really a lot of software build on edifact so edi will be here for long because of this. And for existing systems it is much easier to move to Web Service based architectre if the "core information remains the same" - only fileformat will change from edifact txt file to xml file.
  * Although UBL is much smaller specification than edifact it is large enough to meet the same problems that edifact has today: Parties have to agree what information is deliverd in what fields.
  * Edifact is in our opinion "the best specification" that meets the actual real life requirements at the moment. It is wide but using edifact you can be sure that you are able to deliver your information using it. For us it is sometimes little bit difficult to understand that "squeezing edifact to smaller specification" is only a matter of reorganizing and grouping data in different way. Edifact is quite clever structure and we believe that there are not so much to squeeze. This don't mean that we think UBL is not good, we only think that edifact has implement more widely real world requirements.







