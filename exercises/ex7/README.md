# Exercise 7 - Building a micro-integration in SAP Integration Suite

SAP Integration Suite, Advanced Event Mesh enables organizations to turn integration inside out. This represents a fundamental shift from traditional, system-centric integrationn, where data is pulled out of systems by middleware and moved between them, to a model in which systems proactively publish events, making data available in real time.

![Pic 1](/./images/Picture%201.png)

This also means that some of the responsibilities traditionally handled by middleware shift to the Event Mesh, such as routing, for example. Micro-integrations are a key cornerstone of this concept. These are lightweight integrations with reduced complexity that enable processing at the edge of the mesh (e.g., transforming an event into a model that is understandable for one of the consumers). Reducing complexity in integrations is made possible by the Event Mesh, and one of the key benefits is that it enables faster scalability. New publishers and consumers can easily be added thanks to the lower integration effort.

Micro-integrations can be developed using the Cloud Integration capability of SAP Integration Suite. This will be demonstrated in this exercise.

## Exercise 7.1 Prepare and deploy your micro-integration flow

Go to SAP Integration Suite, and go to **Design --> Integrations and APIs**, then click on **Create**.

![Pic 2](/./images/Screenshot%202025-05-30%20at%2011.08.27.png)

Now name the package **“Micro-Integrations_[YOUR NAME]”**, provide any short description and click on **Save**.

![Pic 3](/./images/Screenshot%202025-05-30%20at%2011.10.20.png)

Now go back to **Integrations and APIs** and select the package **"AEM-Rapid-Pilot-day3"**. This is a package used in the AEM Rapid Pilot Workshop, which includes 5 half days of hands-on workshop in SAP Integration Suite, Advanced Event Mesh. For more information on the AEM Rapid Pilot Workshop, please reach out to your instructors.

![Pic 4](/./images/Screenshot%202025-05-30%20at%2011.12.33.png)

In the package, go to **Artifacts**, select artifact **“AEMLegacyOutputAdapter”**, click on the **action-icon** on the right and choose **Copy**.

![Pic 5](/./images/Screenshot%202025-05-30%20at%2011.14.10.png)

Now a little pop-up pops up, append the name of the artifact with your own name and make sure the package that you just created is selected.

![Pic 6](/./images/Screenshot%202025-05-30%20at%2011.15.28.png)

After the integration flow is successfully copied, you can navigate to the package and go the integration flow.

![Pic 7](/./images/Screenshot%202025-05-30%20at%2011.16.36.png)

Now, we are going to adjust the integration flow, so click on the Edit button.

![Pic 8](/./images/Screenshot%202025-05-30%20at%2011.17.49.png)

First, whether the connection details of your mesh are correct. 

![Pic 9](/./images/Screenshot%202025-05-30%20at%2011.18.50.png)

If the Processing tab is already open, you can continue there. If not, open SAP Integration Suite, Advanced Event Mesh in a new browser tab and follow these steps:
1.	Go to Cluster Manager.
2.	Select your Event Broker Service.
3.	Click Connect.
4.	Under View By, select Protocol.
5.	Choose Solace Messaging → Solace JMS API.
6.	Copy the following details from the connection pane on the right:
    -  Host URI
    -  Message VPN
    -  Username

Paste these values into the connection details for the Advanced Event Mesh Sender Adapter.
Keep the tab with the connection details open, you’ll need it shortly.

Enter a unique password secure alias this must be unique so you can name it:
**BrokerUserPass[YourFirstAndLastName]**.

Then, save your integration flow and go to SAP Integration Suite --> **Monitor --> Integrations and APIs.**

Scroll down until you see the tile **“Security Material”**, then click on **Create --> Secure Parameter**.

Give it the same name as your password secure alias: **BrokerUserPass[YourFirstAndLastName]** and enter the password found in the connection details of the Solace JMS API in SAP Integration Suite, Advanced Event Mesh.

Deploy the secure parameter.

Once you've configured the connection details in your broker, go to the processing tab and set the Consumer Mode **“Direct”**.
Since we'll be subscribing to all created sales orders, enter the following under Topic Subscription: 
**sap.com/salesorder/create/V1/%3E**

![Pic 10](/./images/Screenshot%202025-05-30%20at%2011.23.22.png)

Under topic subscriptions, special characters may need to be UTF-8 encoded. “%3E” refers to the wildcard “>”.
You can set the other processing parameters how you like. More info can be found on the [SAP Help Portal](https://help.sap.com/docs/integration-suite/sap-integration-suite/configure-advanced-event-mesh-sender-adapter).

Now **save** and **deploy** the integration flow.

You can see that the deployment status of the integration flow when you go to **Monitor --> Integrations and APIs --> Manage Integration Content --> All**.

![Pic 11](/./images/Screenshot%202025-05-30%20at%2011.28.23.png)

The status of your integration flow should be **“Started”** and in **green**.

If it’s not, check whether it’s an authentication error, if it is check and adjust your credentials, if it is not, check whether you didn’t use any special characters in your topic subscription.

If it is, we can try whether the integration flow is triggered as soon as a new sales order comes in. Go to SAP Integration Suite, Advanced Event Mesh and go to **Cluster Manager**.

![Pic 12](/./images/Screenshot%202025-05-30%20at%2011.33.26.png)

Select your **Broker Service**.

![Pic 13](/./images/Screenshot%202025-05-30%20at%2011.34.44.png)

Then click on **“Try Me!”**, and click on **“Open Broker Manager"**.

![Pic 14](/./images/Screenshot%202025-05-30%20at%2011.36.00.png)

In the Broker Manager, you enter the **Client Username** and **Client Password** which you found on the previous page and click on **“Connect”**.

![Pic 15](/./images/Screenshot%202025-05-30%20at%2011.37.30.png)

And then you enter **sap.com/salesorder/create/V1/[FirstNameLastName]** and enter some sample sales order data in JSON in the message content and then click on **“Publish”**. Leave this page open as we’ll need it later in the exercise.

![Pic 16](/./images/Screenshot%202025-05-30%20at%2011.39.16.png)

If you now go back to SAP Integration Suite, choose Monitor --> Integrations and APIs --> go to “Monitor Message Processing” --> All Artifacts Past Hour Messages.
![Pic 17](/./images/MonitorAllArtifacts.png)

You should see that the flow you just created was triggered.

![Pic 17](/./images/Picture2.png)

There should be an error related to your SFTP server, you can check that by scrolling down at the right to “Logs”, you’ll see a table with “Runs”, there you can click on “Info” and you should see that the red envelope is at the SFTP Receiver Adapter.

![Pic 18](/./images/Picture3.png)

## Exercise 7.2 Adding converters to the micro-integration flow

Now, let’s add some conversion options. Go to **Integrations and APIs --> Micro-Integrations_[FirstNameLastName] --> AEMLegacyOutputAdapter_[FirstNameLastName]**.
In the process steps of **Cloud Integration**, go to **Transformation**, then select **Converters** and add the following converters to your flow (in this order):
1.	JSON to XML Converter
2.	XML to CSV Converter

![Pic 19](/./images/Screenshot%202025-05-30%20at%2011.48.14.png)

Your flow should look like this:

![Pic 20](/./images/Picture4.png)

Once, the converters are added, you can go in the **JSON to XML Converter** step (double-click), go to **Processing** and **uncheck** "Add XML Root Element".
Next, in the **XML to CSV Converter** enter the following path:

/orderHeader/order**

Also make sure the **XML-based** is entered in the **CSV Header** field.
The **Processing** tab in your two converters should look like this:

![Pic 21](/./images/Screenshot%202025-05-30%20at%2011.50.19.png)

Let’s run the flow, go to the top of your screen and click on **save**, then click on **deploy**.

![Pic 22](/./images/Screenshot%202025-05-30%20at%2011.51.59.png)

When you go to **Monitor --> Integrations and APIs**, then scroll down to **“Manage Integration Content”** and click on the first tile, you’ll see that the integration flow is started.

![Pic 23](/./images/Screenshot%202025-05-30%20at%2011.53.29.png)

There, on the right part of the screen, you can scroll down to **“Log Configuration”**, change the Log Level from **“Info”** to **“Trace”**, this will enable you to trace the content of your integration flow. For more information on this trace option, please refer to the [SAP Help Portal](https://help.sap.com/docs/cloud-integration/sap-cloud-integration/tracing-execution-of-integration-flow).
If a pop-up appears, just press “Change”.

![Pic 24](/./images/Screenshot%202025-05-30%20at%2011.55.51.png)

Let’s run the scenario, go back to the “Try Me!” in the Broker Manager in SAP Integration Suite, Advanced Event Mesh.

Reconnect to your broker, publish to the following topic: **sap.com/salesorder/create/V1/[FirstNameLastName]**

And copy and paste the content from [**“SalesOrder.json”**](https://github.tools.sap/I505867/SAP-IS-AEM-Hands-On/blob/main/exercises/ex7/SalesOrder.json) in the message content, then click on **“Publish”**.

![Pic 25](/./images/Screenshot%202025-05-30%20at%2011.57.27.png)

Now go back to **Monitor-->Integrations and APIs** in SAP Integration Suite, go to **“Manage Integration Content”**, and select your integration flow , under “Artifact Details” you’ll see **“Monitor Message Processing”**, you can click on that. 

![Pic 26](/./images/Screenshot%202025-05-30%20at%2012.03.02.png)

This displays the message processing log for the specific integration artifact.
Go to “Logs” on the right pane and click on one of the links that says “Trace”.

![Pic 27](/./images/Screenshot%202025-05-30%20at%2012.04.19.png)

Now, select the “XML to CSV Converter” in the integration flow and go there to “Message Content”. You will see the incoming message content there and you’ll see that this is an XML.
If you do the same for the “End” event in the integration flow, you will see that this XML has been transformed into a CSV. 

![Pic 28](/./images/Screenshot%202025-05-30%20at%2012.06.00.png)

In the following exercise we will show you how you can easily map fields in SAP Integration Suite through Message Mappings. Go back to the previous screen (“Monitor Message Processing”), select your integration flow and go to “Artifact Details”. There you will find a link that says, “Navigate to Artifact Editor”, you can click on that link. 
There you are taken back to the artifact editor, and now you can start editing your flow by clicking on the “Edit” button.

![Pic 29](/./images/Screenshot%202025-05-30%20at%2012.07.50.png)



## Exercise 7.3 Adding a message mapping to the micro-integration flow

Once you are in the artifact editor, you can choose **“Mapping”** in the menu on top. Select **“Message Mapping”** and drag and drop this step in **between the two converters**. Your flow should look like the one below now.

![Pic 30](/./images/Screenshot%202025-05-30%20at%2012.10.23.png)

Now, select the message mapping and click on the **“Create”** icon.

![Pic 31](/./images/Screenshot%202025-05-30%20at%2013.23.40.png)

Give the mapping a unique name and click again on **“Create”**
Now, click on **“Add source message”** and upload **“[SalesOrderAEMSchema.xsd](https://github.tools.sap/I505867/SAP-IS-AEM-Hands-On/blob/main/exercises/ex7/SalesOrderAEMSchema.xsd)”** as the source message.

![Pic 32](/./images/Screenshot%202025-05-30%20at%2013.26.47.png)

After you've added a source message, click on **“Add target message”** and upload **“[SalesOrderTargetV2.xsd](https://github.tools.sap/I505867/SAP-IS-AEM-Hands-On/blob/main/exercises/ex7/SalesOrderTargetV2.xsd)”** as the target message.

Now you can drag and drop a line from the source message to the target message for the following fields (**important to not make any mistakes here!**):
* orderHeader --> orderHeader
* order --> order
* salesOrderNumber --> orderID
* creator --> creator
* date --> orderDate
* salesType --> salesType
* ordertype --> ordertype
* salesOrg --> salesOrg
* distributionChannel --> distributionChannel
* division --> region
* customer --> buyer
* customerID --> buyerID
* customerName --> buyerName
* **zipCode --> address**
* **street --> address**
* phone --> telephone
* orderItem --> orderItem
* item --> item
* material --> material
* materialType --> materialType
* itemType --> itemType
* orderSchedule --> orderSchedule
* scheduleNumber --> scheduleNumber
* quantity --> units
* uom --> uom

Your mapping should look like the screenshot below, pay close attention, both zipCode and street from the source message are mapped to the address field in the target message. Let’s merge these two source fields into one target field, click on **“Address”** field under buyer.

![Pic 33](/./images/Screenshot%202025-05-30%20at%2013.30.48.png)

At the bottom of your screen, you’ll see the mapping expression configuration. On the left side you have functions, here you can search for **“concat”** and add this to your mapping expression. Also remove any links in the mapping expression.

![Pic 34](/./images/Screenshot%202025-05-30%20at%2013.32.14.png)

After this, you can link the field **“street” to “string1”**, and **“zipCode” to “string2”**, they can be split by a comma (,) and a space ( ), add this (“, “) in the **Delimiter String** (without “”). Then, link **“concat” to “address”**. Your Mapping Expression for the address target field should look like this now.

![Pic 35](/./images/Picture5.png)

Let’s level up our message mapping skills, and choose the field “division” in the source message **OR “region”** in the target message.
This mapping expression is now a simple 1-1 mapping expression. We want to change it to an if-else expression: 
IF division = 01 THEN 
  region = Europe 
  ELSE 
  region = Global
END IF

Now that you know how to build a simple mapping expression, build the following mapping expression (functions we’re using are “constant”, “if” and “equals(string)”):

![Pic 36](/./images/Picture6.png)

Now, let’s test our mapping, click on the button **“Simulate”** on top and upload the file **“[orders.xml](https://github.tools.sap/I505867/SAP-IS-AEM-Hands-On/blob/main/exercises/ex7/orders.xml)”** as test input.


![Pic 37](/./images/Screenshot%202025-05-30%20at%2013.37.28.png)

Then, you click on **“Test”** and you should see the source format being mapped to the target format. Feel free to change some values so you can see how the mapping is behaving.

![Pic 38](/./images/Screenshot%202025-05-30%20at%2013.39.04.png)

After you have tested the mapping and it works how you are expecting it to work, you can close the simulate and click on **“OK”** in the mapping editor.
After this you can **“Save”** and **“Deploy”** your integration flow.

![Pic 39](/./images/Screenshot%202025-05-30%20at%2013.41.48.png)

Once the integration flow is deployed, check if the “Trace” option is still on. 
If it’s not change the log level back to “Trace” by going to **Monitor --> Integrations and APIs --> Manage Integration Content**, then you select your flow, and scroll down to **“Log Configuration”** and change the Log Level from **“Info” to “Trace”** and confirm by clicking **“Change”**.

Once the log level is set to “Trace”, go back to the “Try Me!” in the broker manager of SAP Integration Suite, Advanced Event Mesh.
Reconnect to your broker (if the connection was interrupted) and publish the content from "[**“SalesOrder.json”**](https://github.tools.sap/I505867/SAP-IS-AEM-Hands-On/blob/main/exercises/ex7/SalesOrder.json)" to topic **sap.com/salesorder/create/V1/[FirstNameLastName]**.

![Pic 40](/./images/Screenshot%202025-05-30%20at%2013.50.11.png)

Once the sales order has been published, go in SAP Integration Suite to **Monitor --> Integrations and APIs --> Monitor Message Processing --> All Artifacts Past Hour** and select your integration flow. If you scroll down on the right side of the screen to **“Logs”** and select one of the **“Trace”** links.
There, you can click on the “End” event and then go to “Message Content”, you should see the CSV with the mapped content.

![Pic 41](/./images/Screenshot%202025-05-30%20at%2013.52.08.png)



## Exercise 7.4 Publishing the event as a CSV on an SFTP server

Now, you’ve experienced the low-code development of SAP Integration Suite, let’s make our integration flow really publish to an SFTP server.
Go to the following link, create a test SFTP server and copy the credentials: 
https://sftpcloud.io/tools/free-sftp-server

**!Note! If the host is not "eu-central-1.sftpcloud.io", please inform one of the instructors**

Go back to SAP Integration Suite and go **Monitor --> Integrations and APIs --> Security Material**.

![Pic 42](/./images/Screenshot%202025-05-30%20at%2013.59.00.png)

In Security Material, you can click on **Create --> User Credentials** and enter the following details:
-	Name: SFTP_[FirstNameLastName]
-	Type: User Credentials
-	User: [Username of the free SFTP server]
-	Password/Repeat PW: [password of the free SFTP Server]
Then click on deploy.

![Pic 43](/./images/Screenshot%202025-05-30%20at%2014.00.46.png)

Once your credentials have been deployed, edit to your integration flow in.
Go to **Design --> Integration and APIs --> select your integration package --> Artifacts --> open your integration flow --> Edit**, then remove the existing SFTP Receiver adapter as we will configure a new one from scratch.

![Pic 44](/./images/Screenshot%202025-05-30%20at%2014.02.30.png)

Now, add a new SFTP Receiver adapter by drawing a line between the “End” event and the receiver, a pop-up should appear with a list of receiver adapters, select SFTP.

![Pic 45](/./images/Screenshot%202025-05-30%20at%2014.03.27.png)

Once the adapter is added, go the configuration of the adapter and enter the following configuration options under “Target”:
-	File name: orders.csv
-	Address: [host of free SFTP server]
-	Proxy Type: internet
-	Credential Name: SFTP_[FirstNameLastName]

You can configure the other options how you like.

**!Don't forget to save and deploy!**

![Pic 46](/./images/Screenshot%202025-05-30%20at%2014.04.38.png)

If you now go back to the “Try Me!” of your broker manager in SAP Integration Suite, Advanced Event Mesh and publish an event containing the data of “SalesOrder.json” to topic sap.com/salesorder/create/V1/[FirstNameLastName], you will see that the event was processed successfully and was published as a CSV to the SFTP server.

In SAP Integration Suite, go to **Monitor --> Integrations and APIs --> All Artifacts – Past Hour – Completed Messages**.

![Pic 47](/./images/Screenshot%202025-05-30%20at%2014.06.23.png)

![Pic 48](/./images/Screenshot%202025-05-30%20at%2014.07.29.png)

There, you should see that your integration flow was completed successfully.

If you have FileZilla installed on your computer, you can connect from FileZilla to the SFTP Server following these instructions: 
https://filezillapro.com/docs/v3/basic-usage-instructions/connecting-to-a-server/

Then you should see the CSV file containing the sales orders we sent from SAP Integration Suite, Advanced Event Mesh in FileZilla.

After you have successfully completed all the steps of this exercise, you have seen how you can develop an micro-integration flow in SAP Integration Suite that listens to a topic in SAP Integration Suite, Advanced Event Mesh, maps and transforms the event, and publishes the event to a specific target, in this case an SFTP server.



