# Exercise 7 - Building a micro-integration in SAP Integration Suite

SAP Integration Suite, Advanced Event Mesh enables organizations to turn integration inside out. This represents a fundamental shift from traditional, system-centric integrationn, where data is pulled out of systems by middleware and moved between them, to a model in which systems proactively publish events, making data available in real time.

![Pic 1](/./images/Picture%201.png)

This also means that some of the responsibilities traditionally handled by middleware shift to the Event Mesh, such as routing, for example. Micro-integrations are a key cornerstone of this concept. These are lightweight integrations with reduced complexity that enable processing at the edge of the mesh (e.g., transforming an event into a model that is understandable for one of the consumers). Reducing complexity in integrations is made possible by the Event Mesh, and one of the key benefits is that it enables faster scalability. New publishers and consumers can easily be added thanks to the lower integration effort.

Micro-integrations can be developed using the Cloud Integration capability of SAP Integration Suite. This will be demonstrated in this exercise.

In this scenario, an application will be publishing sales order data events (e.g. sales order created, sales order changed, etc.) to topics in SAP Integration Suite, Advanced Event Mesh. These events will be stored in a queue, and SAP Integration Suite (Cloud Integration capability) will be picking up the events from this queue. In SAP Integration Suite, a micro-integration will convert the format of the events from JSON to a CSV file and will also map the structure of the events to a new target structure. After SAP Integration Suite has transformed the events, it will push the CSV file to an SFTP server.

<img width="420" height="98" alt="image" src="https://github.com/user-attachments/assets/1c3cf53f-7941-4a91-a838-c1b0676aee49" />

## Exercise 7.1 Add new subscriptions to your queue

In the first step, you will make sure that the queue in your broker (created in the previous exercise) is subscribed to sales order topics. This ensures that when applications are publishing sales order events to the broker, they are queued in our queue.

1. Go to your queue in SAP Integration Suite, Advanced Event Mesh by going to Cluster Manager.

<img width="1847" height="825" alt="image" src="https://github.com/user-attachments/assets/d61a0377-0d7e-43af-9a12-a80437dc6ab4" />

2. Select your Broker Service

<img width="1847" height="132" alt="image" src="https://github.com/user-attachments/assets/c1d21551-469e-40b3-889c-af98b3aaafc9" />

3. Click on "Open Broker Manager"

<img width="1847" height="525" alt="image" src="https://github.com/user-attachments/assets/e7ee0130-a93e-4fe2-9c7c-71188de7c8ca" />

4. In the broker manager, go to "Queues"

<img width="1847" height="807" alt="image" src="https://github.com/user-attachments/assets/f308cf03-d9bc-4f66-a829-0288e7ab59b1" />

5. Now, select the queue you created in the previous exercise

<img width="1847" height="109" alt="image" src="https://github.com/user-attachments/assets/e1614b8a-35e6-47a2-9c0e-f87d5acbd335" />

6. Go to "Subscription" and subscribe the queue to different topics by clicking on the button that says: "+Subscription".
7. Add the following subscriptions:
•	sap.com/salesorder/create/V1/>
•	sap.com/salesorder/change/V1/>
•	sap.com/salesorder/retry/V1
•	salesorder/create/V1

Please note, the topics we're using use the wildcard ">".
<img width="1726" height="427" alt="image" src="https://github.com/user-attachments/assets/496cad1f-07a1-42dc-bd3d-6f386e40837a" />

9. Once your queue is subscribed to the topics, we will onboard our publisher. 
We built an application that is publishing sales order events on the topics that we just defined. 
This way, you should see that the sales orders you publish in this application land in your queue.
This will eventually trigger the integration flow that we will build in SAP Integration Suite.

Go to this application by accessing the following link: 

https://solacedemo-uf1dchbp.launchpad.cfapps.ca10.hana.ondemand.com/a31830ca-c8a1-4f4f-936a-eb998f98c20e.CustomerOrderFormRPP.CustomerOrderFormRPP-1.0.0/index.html

To connect to the application, enter the connection details of your broker on top of the webpage:

<img width="1853" height="399" alt="image" src="https://github.com/user-attachments/assets/b339ae2d-4078-402c-ae0c-c969b9b4e837" />

You can find these credentials in the Cluster Manager in SAP Integration Suite, Advanced Event Mesh. Go in AEM to Cluster Manager --> select your broker --> go to the “Connect” tab. 
There you look for “Connect with JavaScript” and select “Solace JavaScript API” which uses the protocol “web-messaging”. Once selected, a pane on the right side of the screen should open with the connection details. Copy and paste these credentials in sales order application.

<img width="1853" height="897" alt="image" src="https://github.com/user-attachments/assets/564c34d9-1c3e-48b8-9939-ad31de781ad2" />

If the credentials are pasted in the sales order application, click on the “Connect” button on top, now you should get the notification that says “Successfully Connected” on the bottom of the page. Once connected, enter sales order data in the order form on the left and click on “Submit”. You should see the sales order appearing in “Order Tracking Status”.

<img width="1710" height="832" alt="image" src="https://github.com/user-attachments/assets/e77ad8ff-9465-465f-91fe-fa9983442a2c" />

Keep the sales order application open in a separate tab, as we will use this application for the remainder of the exercise.

Now, let’s enable SAP Integration Suite to authenticate to your broke, so it can read from your queue. Go back to the “Connect” tab in the Cluster Manager of your broker in SAP Integration Suite, Advanced Event Mesh and on the right top choose View by --> Protocol.

<img width="1858" height="611" alt="image" src="https://github.com/user-attachments/assets/0749fcf5-fa2a-489b-8350-d5a648a667c3" />

There, you select “Solace Messaging --> Solace JMS API and copy the password that you find on your right.

<img width="1522" height="684" alt="image" src="https://github.com/user-attachments/assets/9ea3d501-962f-42a9-bd4e-d6eb1ad45ea7" />

Do not close the connection tab in SAP Integration Suite, Advanced Event Mesh, open a new tab in your browser and navigate to the SAP Integration Suite tenant. 

Now, we will add the credentials of your broker in SAP Integration Suite. Go to Monitor --> Integrations and APIs --> scroll down to “Manage Security” --> click on the tile “Security Material”

<img width="1522" height="718" alt="image" src="https://github.com/user-attachments/assets/d4ca17c9-b148-435d-8cee-6ea106e11f8c" />

In here we can upload and create various credentials, click on the “Create” button on the right top and choose “Secure Parameter”.

<img width="1859" height="292" alt="image" src="https://github.com/user-attachments/assets/82407a45-45d9-4a27-b09b-83ae10d588df" />

Give your secure parameter a unique name, for example SOBroker_FirstNameLastName and enter the password you just copied from SAP Integration Suite, Advanced Event Mesh (twice) and deploy.

<img width="1857" height="848" alt="image" src="https://github.com/user-attachments/assets/028165d5-4525-4cb9-826d-34cf7e0afffb" />


## Exercise 7.2 Prepare and deploy your micro-integration flow

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

First, select the Advanced Event Mesh sender adapter.
Then, copy the “Host URI”, “Message VPN”, and “Username” of your broker and paste it in the sender connection details in SAP Integration Suite. You can copy this from the same page where you previously copied the password from. 
(in AEM: Cluster Manager --> Select your Event Broker Service --> Connect --> Select View By “Protocol” --> Select “Solace Messaging --> Solace JMS API.)

Once this is done, you should enter the name of the Secure Parameter you created (e.g. SOBroker_FirstNameLastName) under “Password Secure Alias”.

![Pic 9](/./images/Screenshot%202025-05-30%20at%2011.18.50.png)

Once you have configured the connection details in your broker, go to Processing and there you can select a consumer mode, there are three options:
•	**Direct**: consumer subscribes to a specific topic, there is no persistence or retry and messages are only delivered while the consumer is connected. Messages are delivered through describing to a topic. Ideal for use cases where low latency is critical and message loss is acceptable.
•	**Guaranteed**: ensures messages are persisted in a temporary queue and delivered reliably while the flow is running. Ideal for use cases where reliability is important during runtime.
•	**Durable Topic Endpoint**: designed for the most mission-critical processes, ensuring no message is lost even during outages, restarts, or redeployments. Perfect for financial data or audit logs where every message counts.

For our use case, sales order processing, we choose the consumer mode **“Guaranteed”**.
Choose the queue you created in the previous exercise as queue name.

<img width="1444" height="848" alt="image" src="https://github.com/user-attachments/assets/950dd4c8-0df4-4a30-a4a9-9849999f4124" />

Now **save** and **deploy** the integration flow.

You can see that the deployment status of the integration flow when you go to **Monitor --> Integrations and APIs --> Manage Integration Content --> All**

<img width="1857" height="816" alt="image" src="https://github.com/user-attachments/assets/8b8c3d34-c925-4b18-9962-062b0adafb05" />

The status of your integration flow should be **“Started”** and in **green**.

If it’s not, check whether it’s an authentication error. In case it is, check and adjust your credentials.

Once the status of your micro-integration flow is started and in green, you can click on **“Monitor Message Processing”** under **“Status Detail”**. This will take you to the message processing monitoring of this specific integration flow.

<img width="1669" height="472" alt="image" src="https://github.com/user-attachments/assets/89f71b6f-8cc0-4fba-bc56-47883f4fae4c" />

You should see that your flow has been triggered and failed, if not try creating some sales order events in the sales order app.

<img width="1848" height="1009" alt="image" src="https://github.com/user-attachments/assets/63d972b1-61ac-4c5f-81c0-822a6dd66723" />

There should be an error related to your SFTP server, you can check that by scrolling down at the right to “Logs”, you’ll see a table with **“Runs”**, there you can click on **“Info”** and you should see that the red envelope is at the SFTP Receiver Adapter.
<img width="1848" height="841" alt="image" src="https://github.com/user-attachments/assets/c523dd01-b518-44cb-8348-92fbef2cfdb5" />

If you go back to the previous screen (Monitor Message Processing), and you select one of the runs, go to “Artifact Details” and click on “Navigate to Artifact Editor”.
This will bring you back to the artifact editor, here you can start editing your flow again by clicking on the blue “Edit” button.

<img width="1847" height="749" alt="image" src="https://github.com/user-attachments/assets/2b032d4a-0ad7-423c-8b49-0d1544831e17" />


## Exercise 7.3 Adding converters to the micro-integration flow

Now, let’s add some conversion options. Go to **Integrations and APIs --> Micro-Integrations_[FirstNameLastName] --> AEMLegacyOutputAdapter_[FirstNameLastName]**.
In the process steps of **Cloud Integration**, go to **Transformation**, then select **Converters** and add the following converters to your flow (in this order):
1.	JSON to XML Converter
2.	XML to CSV Converter

![Pic 19](/./images/Screenshot%202025-05-30%20at%2011.48.14.png)

Your flow should look like this:

![Pic 20](/./images/Picture4.png)

Once, the converters are added, you can go in the **JSON to XML Converter** step (double-click), go to **Processing** and **uncheck** "Add XML Root Element".
Next, in the **XML to CSV Converter** enter the following path:

/orderHeader

Also make sure the **XML-based** is entered in the **CSV Header** field.
The **Processing** tab in your two converters should look like this:

<img width="1524" height="616" alt="image" src="https://github.com/user-attachments/assets/dafa1ecf-67df-4a3a-b4c9-b4e3202cf632" />

Let’s run the flow, go to the top of your screen and click on **save**, then click on **deploy**.

![Pic 22](/./images/Screenshot%202025-05-30%20at%2011.51.59.png)

When you go to **Monitor --> Integrations and APIs**, then scroll down to **“Manage Integration Content”** and click on the first tile, you’ll see that the integration flow is started.

![Pic 23](/./images/Screenshot%202025-05-30%20at%2011.53.29.png)

There, on the right part of the screen, you can scroll down to **“Log Configuration”**, change the Log Level from **“Info”** to **“Trace”**, this will enable you to trace the content of your integration flow. For more information on this trace option, please refer to the [SAP Help Portal](https://help.sap.com/docs/cloud-integration/sap-cloud-integration/tracing-execution-of-integration-flow).
If a pop-up appears, just press “Change”.

Now go back to the monitoring by clicking **“Monitor Message Processing”**. 

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



## Exercise 7.4 Adding a message mapping to the micro-integration flow

Once you are in the artifact editor, you can choose **“Mapping”** in the menu on top. Select **“Message Mapping”** and drag and drop this step in **between the two converters**. Your flow should look like the one below now.

![Pic 30](/./images/Screenshot%202025-05-30%20at%2012.10.23.png)

Now, select the message mapping and click on the **“Create”** icon.

![Pic 31](/./images/Screenshot%202025-05-30%20at%2013.23.40.png)

Give the mapping a unique name and click again on **“Create”**
Now, click on **“Add source message”** and upload **“[SalesOrderAEMSchema.xsd](https://github.tools.sap/I505867/SAP-IS-AEM-Hands-On/blob/main/exercises/ex7/SalesOrderAEMSchema.xsd)”** as the source message.

![Pic 32](/./images/Screenshot%202025-05-30%20at%2013.26.47.png)

After you've added a source message, click on **“Add target message”** and upload **“[SalesOrderTargetV2.xsd](https://github.tools.sap/I505867/SAP-IS-AEM-Hands-On/blob/main/exercises/ex7/SalesOrderTargetV2.xsd)”** as the target message.

Now you can drag and drop a line from the source message to the target message for the following fields (**important to not make any mistakes here!**):
orderHeader -> orderHeader
  salesOrderNumber -> orderID
  creator -> creator
  date -> orderDate
  salesType -> salesType
  ordertype -> ordertype
  salesOrg -> salesOrg
  distributionChannel -> distributionChannel
  division -> region
  netvalue -> netvalue
  currency -> currency
  customer -> buyer
	  customerId -> buyerId
	  customerName -> buyerName
	  zipCode -> address
	  street -> address
	  phone -> telephone
	  country -> address
	  city -> address
	  emailAddress -> emailAddress
	  	email -> email
  orderItem -> orderItem
  	item -> item
  	material -> material
  	materialType -> materialType
  	itemType -> itemType
  	itemDescription -> itemDescription
  orderSchedule -> orderSchedule
  	scheduleNumber -> scheduleNumber
  	quantity -> units 
  	uom -> uom


Your mapping should look something like the screenshot below, pay close attention, zipCode, street, country and city from the source message are mapped to the address field in the target message. Let’s merge these source fields into one target field “address” which contains “street, zipCode City, Country”. 
Click on “address” field under buyer.

<img width="1839" height="661" alt="image" src="https://github.com/user-attachments/assets/4d3839a5-9349-4210-91a4-10bd4a700862" />


At the bottom of your screen, you’ll see the mapping expression configuration. Remove any links in the mapping expression. On the left side you have functions, here you can search for “**concat**” and add this to your mapping expression.

<img width="1839" height="928" alt="image" src="https://github.com/user-attachments/assets/9872d51a-724e-4450-9e93-d8d76dd73e13" />


After this, you can link the field **“zipCode” to “string1”**, and **“street” to “string2”**, they can be split by a space (add “ “ without “” in the **Delimiter String**). 
Your mapping expression should look like this:

<img width="1244" height="651" alt="image" src="https://github.com/user-attachments/assets/537ac8a0-b7ed-42a4-969b-2072b946849e" />

Now, add a second concat function to join **“street”** with the concat we’ve just created (zipCode and city), split this by a comma (,) and a space ( ) in the Delimiter String. Then, a third concat function to join “country” with the other fields, also split by a comma (,) and a space ( ) in the Delimiter String. Finally, link the last concat expression to the field “address”. Your final mapping expression for the mapping field should look like this: 

<img width="1706" height="517" alt="image" src="https://github.com/user-attachments/assets/b796477e-f16b-46f1-8726-4cf5f21abdd9" />

Let’s level up our message mapping skills, and choose the field **“division”** in the source message **OR “region”** in the target message.
This mapping expression is now a simple 1-1 mapping expression. We want to change it to an if-else expression: 
IF division = DV01 THEN 
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



## Exercise 7.5 Publishing the event as a CSV on an SFTP server

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



