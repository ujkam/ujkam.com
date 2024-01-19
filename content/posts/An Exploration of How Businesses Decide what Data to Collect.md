+++
title = 'An Exploration of how Businesses Decide what Data to Collect'
date = 2024-01-01T00:00:00-04:00
lastmod = 2024-01-19T00:00:00-04:00 
draft = false
mermaid = true
+++

This post will be the first of two that discusses how organizational dynamics, stakeholder incentives, and the goals of the business drive decisions about what data is collected and why.  What is discussed is relevant to companies trying to do predictive maintenance on industrial and commercial machines, but it applies to other industries as well.

<!--more-->

In my experience, when people first start learning about things you can do with data, there tends to be a focus on the last part of the process, where one is trying to use that data as part of a hypothetical business decision.  If you are a Data Scientist, this means you focus on using models make an accurate prediction.  If you are in BI person, your goal is to learn how to make a dashboard with useful charts.  If you are a Data Engineer, you learn how to transform raw data a more user friendly form.

There isn't much focus on why this data was generated and stored in the first place, which is the start of this data process.  Data isn't collected just because we have the technological capabilities to collect it.  Data is collected because people made the decision that collecting this data would be useful, and what it meant to be "useful" was translated into technical requirements for what data needed to be collected.

What people mean by "useful" is not static.  Different stakeholders and organizations have different goals and can have very different ideas, sometimes opposing, of how the same dataset can be useful to them.  And as companies change, and technology evolves, this can also change what people find useful.  All of these stakeholders and changes feed into what people want to do with data, and what data needs to be collected to satisfy this.

This post will be the first of two that discusses the organizational dynamics I've seen with companies trying to do predictive maintenance.  The overall goal is to try and provide some insight into how different stakeholders with different business motivations can influence what data is collected and how it's used.  To get there, it's useful to look first at the architecture of how data might flow in a predictive maintenance project and then make this more concrete using examples of what data is generated and collected on a machine.

To avoid the this post turning into a novella, I will break this into 2 parts.
1. This first post will present some data architectures as a framework to explore how different organizational structures can drive what data is collected and how it's used.
2. Deciding What Data to Collect.  The second post will provide more concrete examples of what data people might want to collect from a machine, and their motivations for doing so.  It'll be more technical, but I think this will provide a lot of context to the first post regardless of if you care about technical details or not. 

I don't think there's an absolutely correct way order these posts, but I thought it made sense to start with architecture first.  If you prefer to jump from here to the second post and then come back to read the rest of this post, that shouldn't be an issue.
# Introduction

In this post we'll look at the data architecture(s) and data flow(s) of a typical use case seen with industrial machines.  This post is focused on the situation where there are machines generating data in a remote location (e.g. a factory) and there is a person or group of people who need centralized access (e.g. in a corporate office) to this data. An alternate to this is somebody who is at the same location as the machine and is directly collecting and analyzing data straight from the machine, but this post isn't focused on this latter type of use case.

On the surface, this data flow is quite simple.  What I am really trying to communicate is the complexity that arises from different organizations owning the data, and different stakeholders having differing incentives and ideas about how to extract value from this data.  So try to avoid taking the architecture diagrams at face value, and instead mentally map them to the organizational abstractions they represent. 

Somebody reading this post might have a lot of questions about the technology choices and data products used in this architecture, but that's not really the point of this post.  The architecture is just the framework to help understand how this data is owned by different organizations in any given company.   If it helps you to visualize things, pick your tool(s) of choice and fill the boxes with those.  

# Architecture Overview

Let's start with a very simple data architecture of machine data.

<div class="mermaid">
flowchart LR
	M[Machine] --> C[Raw Data Landing Zone] --> DW[Data Warehouse] --> DS[Data Scientist]
</div>

What do each of these boxes mean?

1. A **Machine** Generates Data.  This data is stored locally in a database and/or files.  Machine data might be
	1. Event Data, which has data from known events with established events codes (e.g. event code 03040506 means the a robot arm picked something up)
	2. Sensor Data
	3. Images/Audio/other non-textual data.
2. A software agent on or connected to the **Machine** uploads this data to a server somewhere in the form of structured (e.g. XML) or unstructured files. These raw file are securely copied to a server we'll call a **Raw Data Landing Zone**.
3. The raw files are processed and exposed in a **Data Warehouse.**  An example of this is if data from machines around the world are combined into one table that users can query.  Typically some type of automated Data Engineering occurs between the **Raw Data Landing Zone** and the **Data Warehouse**

The diagram above is a very basic representation of a use case where a machine is generating data and somebody somewhere is using that data.  Let's add a bit of complexity by adding non-machine data sources.  "Non-machine" data is data that can be linked to that machine, but is not generated by that machine. 

<div class="mermaid">
flowchart LR
	M[Machine] --> C[Raw Data Landing Zone] --> DW[Data Warehouse] --> DS[Data Scientist]
	S[Service Data] --> DW
	Q[Quality Data] --> DW
</div>

4. In addition to data from the machines, we also have **Service Data.**  This is data created by service people and technicians who service these machines.  This type of data has both structured and unstructured components.  Examples of the structured parts of this data might be
	1. When somebody opened a ticket due to a machine issue
	2. When a service technician started and stopped working on the problem 
	3. Some keywords organized in hierarchical list describing the problem (e.g. Subsystem B -> Fluid Pump -> Leaking Outlet Pipe -> Replacement)
	4. What work was done and how long it took
	5. What parts where changed
	The unstructured parts might be typed up notes documenting every single interaction with the customer, and also a way for the service technician to describe their work beyond what is possible to record in the structured data (e.g. entered using a form).  An example of this might be "Met with machine operator who said this is an intermittent issue, but is happening with increased frequency.  No issues reported in self-diagnostics after running it 2 times, but issue occurred as we were observing normal operation."  The actual text might not be this clear and there are likely to be multiple languages in the data at a global company.
5. **Quality Data.**  Either before, during, or after the machine process, some measurement of quality is performed.  Sometimes this process is done manually by a person, so you only have snapshots of quality.  Some machines can also automatically perform quality checks, but these checks may slow down the machine process and it may not be done every single time.  It's also possible the quality check results were recorded on paper and then entered into a computer later, which can lead to quality issues.

Note that in the diagram the **Service** and **Quality** data are ingested directly into the **Data Warehouse.**  This is because the data is typically entered using a computer and is stored in a local  or cloud database.  So there may be a way to query the data directly and feed it into the Data Warehouse and have no need to temporarily store them in the **Raw Data Landing Zone**

# Data Ownership

Instead of looking at the data flow purely in the sense of how the data flows, let's modify this to include the "owners" of the data.  "Owners" are the business units who define the requirements for what data needs to be collected so they can be use it to run their part of the business.  These are the ones who say "I need this metric/KPI, and this is the data I need to calculate that."  

In most companies you'll also see another form of data ownership, which is who owns the data and data pipeline from a technical standpoint.  For example, if you query a database and the data has an error, who do you contact to fix that?  There's typically a technical team that is responsible for those types of issues.  This post is not about this latter group of technical owners, but really about the business owners.

## A Single Company Owns Everything

Let's assume all the business and technical owners/organizations are at a single company called Company A that designs, makes, and runs the machine(s).  Everything is internal and there are no external vendors who own these functions.

The "owners" are shown in the chart below.  To make this easier to read, the boxes with red borders are the business owners.  The other teams work with the data and build things (e.g. Dashboards) with it, but ultimately the business owners are the ones using this data to reduce costs or increase profits

<div class="mermaid">
flowchart TD
	subgraph Company A
		subgraph Engineering
		M[Machine]
		end
		subgraph Service
		S[Service Data]
		end
		subgraph Quality
		Q[Quality Data]
		end
		subgraph IOT[IoT Group]
		C[Raw Data Landing Zone]
		end
		subgraph DT[Data Team]
		DW[Data Warehouse] 
		end
		subgraph A[Analytics]
		DS[Data Scientst]
		end
	end
    style Engineering stroke:#ff0000
    style Service stroke:#ff0000
    style Quality stroke:#ff0000
	M --> IOT
	DW --> A
	S  --> DT
	Q --> DT
	C --> DT
</div>

**Important:**  It's important to recognize something here about data ownership.  Each organization defines their data requirements based on what they need to run their part of the business.  This is in contrast to an org like the the **Analytics Org**.  People in the **Analytics Org** aren't defining what data needs to be collected, they are using data that was generated because of the requirements defined by another part of the company.  In other words, people in the Analytics org are using data that is a byproduct of some business process, they are not defining why that data needs to be collected in the first place.  That being said, it is possible that somebody in the Analytics org will specify additional data that needs to be collected to support a business use case.  And then the business unit that asked for that use case will justify to somebody else (e.g. Engineering) why they should collect that data.  But even in this case, the Analytics org doesn't "own" that requirement.  It's the business unit that owns it.

To make this concept of ownership more concrete, let's take the **Service** org as an example.  Service is responsible for ensuring the machines are running properly.  If the machine is not running properly somebody will contact the service organization to fix the machine.  The service org provides these basic functions
- Call center personnel and to interact with the machine operators and other personnel at the customer(s)
- Remote and onsite troubleshooting 
- Remote (if possible) and onsite fixes
	- Scheduling onsite visits
	- Dispatching technicians to the machine site if an onsite visit is needed
- Parts inventory and ordering parts

From a data requirements perspective, this translates into
1. Recording Customer Contact information and machine (e.g. the machine serial number) details
2. Tracking of machine operator/customer interactions with customer support
3. Documenting what repair/service work was done on the machine and who did it
	1. What work was performed and date/time when it was done
		1. Work time(s) and travel time(s) for each time work was done
	2. Parts used or changed
4. Identification of which system or part in the machine was causing the issue 
5. Overall tracking of a Service Ticket
	1. Open (when the service department was alerted to the issue) and close (when the issue was resolved) date and time.
	2. Total time spent on work and travel
	3. Total monetary cost of this service ticket to the customer and the service org

The list above would be provided to a technical team as the requirements for an application or  software service(s) where this data can be collected (e.g. using a web form), viewed, modified, stored, and exposed to people inside and outside the service org.

Note that the service org might not collect and store Machine Data (e.g. sensor data) as part of their daily operations.  However, they may need machine data to understand the behavior of the machines so they can use it to diagnose problems.  So instead of storing this data in their own systems, they may get this data from the Data Warehouse.

## Ownership Spread Across Multiple Companies

The nature of ownership and incentives can change quite drastically if all of these functions are at different companies.  Let's change the structure now so that different companies or service providers own a different piece of this diagram.

First, let's assume that the Machine is built by a **Machine Builder,** which is a company that builds machines that somebody else uses.  A basic example of this is something like a commercial water heater.  The company that makes these water heaters probably sells millions of them per year.  However, while they design and manufacture the water heaters, they aren't heavily involved in installing or servicing them. Outside of identifying potential product defects that effect a large percentage of customers, they aren't interested in collecting near real-time data from each heater.  Other examples of machines built by **Machine Builders** include CNC Machines, Electrical Submersible Pumps used in Oil & Gas, Electric Vehicle Battery Testing Equipment, etc. 

In the diagram below, each company is a different color.  You can contrast this to the single company chart above, where all the business functions are owned by the same company.  To make it easier to understand what each company is doing, I've modified the text in the boxes to describe what they are doing.  The data is a by-product of this (e.g. Servicing machines creates service data).

<div class="mermaid">
flowchart TD
	subgraph MB [Machine Builder]
	DSM[Designs and Sells Machine]
	end
	subgraph Customer
	M[Uses Machines]
	Q[Measures Quality]
	end
	subgraph TPS[Third Party Service]
	S[Services Machines]
	end
	subgraph ITSP[IT Service Provider]
		RDLZ[Collects Raw Data]
		DT[Performs Data Engineering]
		DW[CreatesData Warehouse]
	end
	subgraph AC[Analytics Company]
	DS[Builds Analytics]
	end
	style MB fill:#9fc5e8
	style TPS fill:#ffd966
	style Customer fill:#8fd24b
	style ITSP fill:#e69138
	MB -. Sells Machine To .-> Customer
	Customer --> ITSP
	S --> ITSP
	Q --> ITSP
	ITSP --> AC
	RDLZ --> DT --> DW
	AC-. Provides Analytics .-> Customer
</div>


To understand what's going on in this diagram, let's assume the Machine Builder sells some sort of complex machine for manufacturing.  There is a customer who buys many of these machines and runs them in factories across the world.  The machines are serviced by different companies depending on location.  As part of the customer's contract requirements with the service providers, they ask that their service data be made available.  The customer decides they want to provide more insight into their factory operations, so they pay an IT Service provider to collect and process all the data and make it available in a centralized data warehouse.  The customer also contracts with an Analytics company to provide some sort of Analytics (e.g. Dashboards and KPIs) they can use to monitor their machines and improve operations.

There are lots of variations of the diagram above.  Maybe the Machine Builder sells an IoT subscription where each machine automatically uploads data to a cloud service and the customer can access it.  In that case, maybe the IT Services Provider is not required and the Analytics company can directly access that data.

It could be that the Service company and the Machine Builder are two separate companies under the same international conglomerate and they work together to provide Analytics.  When the customer buys the machine they get some basic pre-built dashboards for no additional cost, but there is also a subscription that provides more comprehensive service dashboards and other features (e.g. notifications).

How these different companies (boxes) are organized and who offers what service can change the incentives around what data is offered.  If the Machine Builder only builds and sells machines, their machines might be designed to expose the bare minimum of data.  If the Machine Builder sells an IoT service, they might be motivated to collect and provide more data, but at different data subscription tiers.  If the Machine Builder and the Service company are closely aligned, maybe they will have access to data that the Analytics company will not get access to.

The customer is not automatically passive in all this.  A very large customer might say they need the machine builder to update the machines to expose some data.  If the machine builder says no, the customer might start considering a different machine builder for their next factory.

# The Incentives around Collecting Data

Realistically, nobody starts with the idea of collecting and exposing as much data as possible just for the sake of doing it.  Collecting and exposing data has costs in terms of the time to build the functionality, the hardware and software people needed to implement it in the machine, and the financial costs to do it.  Somebody has to conclude that it's worth it to expose that data.  For a company and the business org under it, that usually means making that data available will increase profits by increasing sales or driving down costs. 

Whether we are talking about all the business functions being under a single company or across multiple companies, it's important to realize that the incentives between different business functions motivate the decisions around what data to expose.  Rather than provide generalities, I'll provide examples.
- An Engineering (Machine Builder) org focused their "post-sale" troubleshooting efforts on identifying the parts of the machine that were breaking and driving up warranty costs.  To identify the root cause of the issue, they identified the steps (using the Error log) that led to the issue.  For example, Step A -> Step B -> Step F is more likely to lead to issue than Step A -> Step B -> Step D.  Another name for this is Root Cause Analysis by using a Causal Tree.  By recreating this in their lab environment, they were able to identify what was going on and if it was more pragmatic to update the software/hardware or eat the warranty cost.
	- However, the Service Org was seeing many other machine problems that could not be diagnosed using this method.  They needed sensor data to help determine what was going on.  But since the Engineering group felt their troubleshooting methodology was sufficient for their needs, they consistently deprioritized adding new sensor data to the machine data exports, which hindered the Service Org.
- An Engineering Group wanted to add a lot of telemetry to their product, but there were concerns about legal risks.  After classifying their use cases as risky or less-risky, it was not clear there was a good business model for only implementing the less-risky use cases.  In other words, there was not much value to the customers or the company to add that telemetry.
- A Service Org wanted to move from reactive (e.g. fixing an issue after it occurs) to proactive (identifying and alarming on possible issues before they occur) service.  However, due to budget limits on staffing, they couldn't realistically act on a proactive alarm in less than 96 hours.  Based on this, they where not really motivated to make the business case to engineering to increase the data frequency or what data was collected (e.g. instead of just a daily average, have an hourly minimum, maximum, average, etc)
	- In a more dysfunctional company, this is also something that can be weaponized.  For example, the service org can be accused of "Not being responsive to proactive issues" even if they can't realistically respond to them because of limited personnel.  In that case, the service org might actively discourage any improvements in data collection.
- Instead of talking about Machine Data, here is an example from Service Data.  When service personnel went onsite to a customer, they were required specify (in the service ticket) the part of the machine they worked on and what was broken.  However, in many cases, it was not 100% clear what the root cause of the issue was, resulting in multiple parts being changed.  So the data that was entered into the service ticket reflected what was changed, but not what the cause of the breakage was.  From a Data Analysis and Predictive Model perspective, this added label noise because it was hard to determine actual reason for the issue.  Fixing this would require updating the service software + UI used by the service personnel, and retraining all the service personnel on the new system.  Looking at this purely from the perspective of what value cleaner data and better analytics would provide if they made this change, it was not clear that it needed to be done.
	- As a real world example of this, there was a water pipe that was routed over a circuit board.  If that pipe leaked, it would leak directly onto the circuit board and cause a failure.  Since the circuit board was much more expensive than the pipe, the former was typically entered as the "cause."
		- This was the type of issue where implementing an engineering fix (e.g. moving the pipe) would provide much higher ROI than adding a bunch of data to predict this issue.
- There are machine issues where fixing something after it breaks is the best course of action.  For example, there might be a 50 cent rubber gasket that leaks, but it takes 6 hours of labor to replace that gasket.  In that case, it might not be worth it to collect a lot of data to try and predict when it is going to fail.  Instead, you replace it every year as part of a yearly scheduled maintenance because it isn't worth it to optimize when that part should be changed. 
- An Engineering and Services Org was struggling to clearly define which parts should be reengineered (e.g. like a recall), which should be covered under warranty, which should be fixed by service, and which should just be left to break and fixed reactively (customers without a service plan would have to pay for it themselves).  In other words, who should pay for the fix?  Since they all agreed that the current confusion was untenable, it was much easier for all business owners to make the case (and find budget) for better data so they could better classify how issues should be handled.

As you can see in all these examples, the goals of all the business owners define how motivated they are to push for data collection and data quality improvements.  And it's not just each business owner themselves, but these motivations are also driven by how collaborative or adversarial these relationships are between these owners.  Every situation is different, and can evolve (or devolve!) over time.
# How does this impact Analytics and Data people?

As somebody who works in a data analytics role, data issues can be really frustrating and you can feel like screaming "Can't you see this change with the data is needed?  Why is it so difficult for you to understand?  You are the one who asked me for this use case, and this is what needs to be done to be successful."  It's not always clear why some data is not collected/exposed or why some major data issue is not taken seriously and addressed.  

One thing that has helped my sanity in these situations is to defocus from the particular project I'm working on and think about the business motivations.  Many times, this provides the perspective needed to understand why a change is not going to happen and the real answer is there is no business justification for resolving this.  And in many cases, if a fix requires a jump from one ownership box to another, that's one box too far. 

If you are pushing for a change, instead of beating people over the head with more analysis and trying to convince them you are right, step back and look at motivations and business use cases.  People understand "The impact of not resolving this data issue is $100K" a lot more than 50 slides of data limitations and the issues it causes.  In doing so you might realize the fight you're having isn't worth the effort you are putting into it.  I've certainly been in a situation where the expected ROI of a project was very high ($100K+/year), but after some analysis we realized that with the use case defined the way it was, that number was totally unrealistic and that number was revised down quite a bit and the project was put on hold.

Unfortunately, it might take a long time to be able to see these business motivations, especially as a new employee or with new stakeholders.  You also need to form the right relationships with the right people to really understand what is driving decision making.  But I think it's worth the suffering for you to build that knowledge and those relationships.

At this point I hope you have a basic understanding of how data flows and how different business owners can drive what data is collected and how it's used.  In my next post I'm going to provide more concrete example of machine data, and also use this to provide more examples of how business requirements drive and define what data to collect
# Final Thoughts

To wrap things up, let me just reiterate some key takeaways.  Data is collected because somebody thought it would be useful to collect, not just for the sake of collecting data.  In a commercial context, the people who define what is useful are business owners, who want to use this data to reduce costs or increase sales.  The business owners are motivated in different ways, and it's important to keep their motivations and incentives in mind when trying to understand what data is collected and why they find it useful.  Also remember that different business owners have to work together, and how aligned their goals are influences how they perceive each other.  

One last point.  Over time, the cost of collecting data goes down, and the need to collect data goes up.  A 30 year old machine might not expose any data at all, and the only way to even get data from it would be to physically add sensors and electronics to capture and export that data.  In 2023,  unless a machine is extremely simple, it's likely a machine will collect some data, even if it doesn't give customers access to that data.  Compared to 30 years ago, there's no need to convince anybody that collecting data from a machine is something that needs to be done.  It's really more a question of what data needs to be collected and how to make money from that data.




