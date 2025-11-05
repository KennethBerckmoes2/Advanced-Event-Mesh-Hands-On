# Exercise 6 - Advanced Event Mesh as an end-to-end platform for Event-Driven Architectures

In this exercise, you will decide yourself what you would like to check on. Find below a number proposals of what you could do with Advanced Event Mesh. 

![Pic 1](/./images/ex6-1.png)

## Exercise 6.1 Explore Insights

After completing these steps you will have looked into Insights

Insights allows you to use monitoring dashboards and configurable notifications to detect potential issues before they negatively impact your brokers and event broker services.

Hint: the Datadog features might be disabled. If you want to see these, check with the instructor.

### Background

Insights enables operations and applications teams to ensure their event broker service and event mesh infrastructure are available and ready for use by business applications, providing a centralized, at-a-glance view on the availability of various aspects such as:

- resource usage
- event mesh health
- message flow
- High-Availability (HA) status
- queue, topic endpoint, RDP, and bridge health
- message spool utilization
- capacity utilization

Using Insights provides a turnkey monitoring service that you can subscribe to. The service helps you build an understanding of your estate — that is, your deployment of event broker services, the messaging and activities that occur, and any capabilities that are part of your Advanced Event Mesh for SAP Integration Suite account (Workspace). Insights helps you to:

- Keep your applications and event broker services running optimally as key performance indicators (KPIs), metrics, and events help you understand performance and usage, and proactively detect and address issues in your event-driven architecture (EDA).

- Gain a better understanding of application behavior and size your event broker services — all from one place. You can identify misconfigurations, visualize how events flow between applications, monitor your applications more effectively, and evaluate the overall performance of your EDA.

### Steps

1. Go to Advanced Event Mesh

2. Select Event Insights

![Pic 2](/./images/ex6-2.png)

3. Look at the different pieces of information that Event Insights provides you

![Pic 3](/./images/ex6-3.png)

To learn more about the metrics and monitors that Advanced Event Mesh offers, click on the respective links.

Please note that the main tool, Datadog, has not been enabled for this session.


## Exercise 6.2 Event Management

Explore event management as part of Advanced Event Mesh by looking into the Event Portal section.

### Background

In recent years, more organizations have started to use and share APIs. At some point, these organizations realized that they needed an API management solution to help maintain governance over their APIs — such as SAP API Management in SAP Integration Suite. With event-driven architectures (EDA), it’s not much different.

The more you use EDA, the messier it becomes. There will be many queues, topics, events, publishers, and subscribers across the organization. So how do you monitor that? How do you manage it? And how do you maintain governance?

This challenge is addressed by the Event Portal of SAP Integration Suite, Advanced Event Mesh. The Event Portal is a cloud-based event management tool that enables organizations to:
1.	Discover and design their event-driven architecture (EDA) through event flows
2.	Authorize who can consume events
3.	Manage the lifecycle of events (promote, deploy, version, etc.)
4.	Audit what is running in production vs what is designed
5.	Improve the existing EDA setup

It is a collaborative platform where various personas like architects, developers, middleware teams and business analysts typically have their own responsibilities in defining and using the EDA setup.

![Event Portal Intro](/./images/EventPortal1.png)

A key benefit of using Event Portal is its ability to track the relationships that exist within an extremely decoupled event-driven architecture (EDA). It enables the reuse of schemas and events and provides a graphical view of the relationships between applications and events.

Furthermore, as an Event Portal user, you can model your event-driven architecture (EDA) across different operational environments. Event Portal allows for environment separation, similar to how event broker services are separated within enterprises. This means you can create a different Cloud Console or Event Portal account for each environment within your enterprise. You can then define and grant different user access permissions for each of these environments, allowing you to model your EDAs as they progress from one environment to the next.

The Event Portal is also a very open platform. It can be integrated with tools such as the Developer Hub in the API Management capability of SAP Integration Suite (to share event documentation with internal developers and/or business partners), as well as non-SAP tools like Slack, Confluence, GitHub, Jenkins, Ansible, Dynatrace, and others.

Organizations that want to roll out EDA are running blind without an Event Portal.


### Walkthrough Event Portal

Let’s walkthrough some of the features of the Event Portal


1.	Go to the home of SAP Integration Suite, advanced event mesh and open “Designer”
![Event Portal](/./images/Portal1.png)

2.	Here, you find different application domains. Application domains are grouping different EDA setups and are typically structured based on the line of business (LoB) like sales, logistics, HR, etc. or business unit. In this example, it is structured on the different LoBs, let’s look in “Sales”. 
![Event Portal](/./images/Portal2.png)
3.	Now, we see an overview of the event flows. We see which applications are part of the setup (e.g. ERP System, Process Automation, UI5 Business Partner Dashboard, etc.) and how different events are flowing between these applications. This is typically defined by business analysts and architects. 
![Event Portal](/./images/Portal3.png)
4.	Click on the “Sales Order Change” and “Open Full Event Details”. 
![Event Portal](/./images/Portal4.png)
5.	On the right side of the screen, we see all details related to the event, including the version, the lifecycle state (Draft, Released, Deprecated, Retired), the topic taxonomy, the Schema and an overview of which applications are subscribed to the topic and publishing to the topic. Feel free to browse through the different details.
![Event Portal](/./images/Portal5.png)
6.	On top, we can see that the event is not shared. This means, it is only related to the application domain “Sales”. When an event is shared, it can be reused across different application domains.
![Event Portal](/./images/Portal6.png)
7.	On the left side, you can see the current version and the version history of the event. Let’s make a new version by clicking on the plus in version. 
![Event Portal](/./images/Portal7.png)
8.	Give this version your own name, a description and give it the following Topic Adress: sap.com/salesorder/change/V*/{salesOrg}/{distributionChannel}/{division}/{customerId} (With * being your version number).
![Event Portal](/./images/Portal8.png) 
9.	Select the “Sales Order Schema” as Schema
![Event Portal](/./images/Portal9.png)
10.	Click on “Save Version”, now you have created your own version of an event.
11.	Go back to the visual representation of the application domain.
![Event Portal](/./images/BackToVisualRepresentation.png)
12.	In the visual representation of the application domain, you can now see your new event version. The dotted line around your event means that the event is still in draft. This allows you visually add publishers and subscribers by dragging arrows between your application and the event (we will not do that in this exercise). It also enables you to visually track which applications are subscribed to which version of which event.
![Event Portal](/./images/Portal10.png)
13.	You can also change your different components like Applications, Events, Schemas, Enumerations, etc. through a list view. Click on “Catalog” in the left menu and browse through the different catalog components. 
![Event Portal](/./images/Portal11.png)
14.	On the left, you can also find the KPI dashboard. This enables you to keep track of how events are shared between domains and how events are being consumed. Which allows you to optimize your EDA setup. 
![Event Portal](/./images/Portal12.png)
15.	Now, let’s clean up the Event Portal. Go to Catalog -> Events and select one of the Sales Order Create Events in the list, click on the three dots on the right and click on "Open in Designer". Make sure your event is the one used in the application domain "Sales".
![Event Portal](/./images/Portal13.png)
16.	Select your version and click on “Delete”. 
![Event Portal](/./images/Portal14.png)
17.	A pop-up appears, so click again on “Delete version”. 
![Event Portal](/./images/Portal15.png)


## Summary

By now, it should be clear that SAP Integration Suite, advanced event mesh is more than just an event broker. That it is even more than an interconnected mesh of advanced event brokers. It is an end-to-end solution to bring EDA in a structured and governed way to your organization. It comes with the capabilities to create and manage event brokers, queues and topics end-to-end, collaboratively manage your EDA setup and to monitor the setup in detail.

Please continue with - [Exercise 7 - Building a micro-integration in SAP Integration Suite](../ex7/README.md)



