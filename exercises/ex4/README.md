# Exercise 4 - Consume SAP S/4HANA Event via Advanced Event Mesh Topic

In this exercise, we will consume an event on a second broker via a topic using the Try Me ! tool that comes with Advanced Event Mesh.

## Exercise 4.1 Mesh Manager  

By now, you have already explored some of the different capabilities of Advanced Event Mesh and before the end of the exercises it will be clear what the "Advanced" in Advanced Event Mesh stands for.
As you will experience, it’s a platform that provides advanced features such as advanced monitoring, event management, message replay, filtering, high availability, etc. 

But perhaps you are wondering what the meaning behind “Event Mesh” is?

This is because the solution offers the ability to interconnect brokers. You can create multiple brokers using SAP Integration Suite, Advanced Event Mesh and deploy these brokers on a location of choice, it can be in a region of a hyperscaler (public cloud) or it can be in a local environment (private cloud/on-prem). All these brokers can be interconnected with one another; we call this setup an event mesh. 
There are many reasons to consider creating an event mesh, one of them is performance. 
Communication between brokers is highly optimized. 
This means that if you have applications running in Belgium that need to send events to applications in Canada, it’s much more efficient to deploy a broker in Belgium and another in Canada, link the two brokers, and let the Belgian applications publish to the Belgian broker while the Canadian applications subscribe to the Canadian one.
This approach is significantly faster than relying on a single central broker.

Although this example describes an intercontinental setup, the same principle applies to continental scenarios as well. Organizations in the retail for example, sometimes have brokers running in each of their stores to offer each store performant, highly available, and guaranteed messaging.

Sometimes organizations have non-SAP brokers such as Kafka and Microsoft, which they also need to interconnect and eventually they end up with complex setups. This challenge is also solved with AEM since you can easily connect your non-SAP brokers to the mesh.
In this chapter we will create a simple event mesh.

### Steps

1.	Go to Advanced Event Mesh
2.	Select Mesh Manager.
![Mesh Manager](/./images/MeshManager1.png)
4.	Here, you can see that there is already an event mesh created named “MyMesh”, open this event mesh.
![Mesh Manager](/./images/MeshManager2.png)
5.	Here we see an overview of the event mesh, as you see it includes a broker in the MS Azure public cloud in Amsterdam and Frankfurt. 
6.	Click on “Edit Event Mesh”.
![Mesh Manager](/./images/MeshManager3.png)
7.	Click on “Add Service”.
![Mesh Manager](/./images/MeshManager4.png)
8.	Select the broker you created before and click on “Add Service”.
![Mesh Manager](/./images/MeshManager5.png)
9.	Now click on “Update Event Mesh”, if a pop-up appears just accept and continue.

If all went well, you have added your broker now to the existing event mesh.


## Exercise 4.2 Consume events via a topic on a second broker

After completing these steps, you will have consumed events via a topic from another broker in the Event Mesh. You will use the Try Me! feature of Advanced Event Mesh for this.

To put this into perspective: AEM provides an intelligent distributiuon of events throughout the mesh of Event Brokers. This is what you experience here. The event will be made available wherever there is an interested consumer.

1. Go to the cluster manager and select another broker in the mesh.

![Pic 1](/./images/ex4-1.png)   

2. Click "Try Me!", copy the credentials to a notepad and click on "Open Broker Manager" 

![Pic 2](/./images/TryMe.png)   

3. In the Subscriber section, click on show advanced settings

![Pic 3](/./images/ex4-3.png)   

4. The data in the Establish Connection section should be pre-filled. If not, fill the data in the Establish connection section. You can get this data in the connect part like before. Go to Connect and the Solace Web Messaging section. 

5. Click on Connect

![Pic 4](/./images/ex4-4.png)  

The icon behing Establish Connection should change from Red to Green.

6. Fill your topic into Subscribe --> Add Topic

Your topic is: s4connect 

![Pic 5](/./images/ex4-5.png)   

7. Click on Subscribe

You should see your topic in Subscribed Topics

![Pic 6](/./images/ex4-6.png)   

8. Wait for the instructor to trigger an event in the S/4HANA system

9. Once an event is triggered, it should appear in the messages section

HINT: Remember how your queue buffered events for you? This is not the case here. There is no buffering. Only events triggered while you are listening end up here. A topic works like a radio: you need to have your radio switched on in order to listed to the news.

## Exercise 4.2 Produce and Consume events via a topic 

You might have already noticed that there is not only a Subscriber section in "Try Me!", there is a Publisher Section as well. If you want to, you can try to publish a few events or messages as well. These events go via the brokers.

1. Go to the publisher (enter Client Username and Client Password) and click on connect

![Pic 7](/./images/ex4-7.png)   

2. Enter topic_OrgName_FirstName_LastName (replace Orgname with the name of your organization, FirstName with your first name and LastName with your last name).

Please do not publish to the topics we had used before in order to not spam the other particpants.

![Pic 8](/./images/ex4-8.png)   

3. Add your topic to the Subscriber topic - add the topic into the Add Topic field anc click Subscribe

![Pic 9](/./images/ex4-9.png)   

4. Click Publish in the Publisher.

![Pic 10](/./images/ex4-10.png)   

5. You should see your message in the Messages section of the Subscriber

![Pic 11](/./images/ex4-11.png)   

## Summary

You've now seen what a mesh looks like in Advanced Event Mesh. You consumed the events via a topic on an additional broker in the mesh. The forwarding of events has happened automatically based on your interest into the topic.

Continue to - [Exercise 5 - Explore Topic Hierarchies and Wildcards (optional)](../ex5/README.md)
