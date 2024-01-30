+++
title = 'Deciding what Data to Collect'
date = 2024-01-23T00:00:00-04:00
draft = false
mermaid = true
+++

In my [**previous post**]({{< ref "An Exploration of how Businesses Decide what Data to Collect" >}}), we looked at how stakeholders and organizational structures influence decisions about what data is collected and exposed for use.  If you haven't seen machine data before, that post might have felt a little abstract and some things might have felt contradictory (e.g. how does one look at the events before a failure without any sensor data?).  For people who prefer concrete details and real examples, this post is for you. 

<!--more-->

When I was involved in my first predictive maintenance project, I had a fairly naive view of machines and the business of deriving value from machine data.  In my mind there was a machine that generated a lot of data and you analyzed that data.  I didn't really recognize or think about topics like
- What's the bare minimum data actually needed to run this machine?
- What data is not needed, strictly speaking, to run the machine, but is necessary for better diagnostics?
	- What additional data is required for predictive maintenance?
- What is the cost and effort required to get more data from the machine?
- Does the cost of a predictive model even make sense?  Will the model cost more to build and operate than it saves the company?

A good starting place is to understand how a machine works and what data can be expected from it.  If we start from the basics, we can gradually build an understanding of how people define the requirements for what data should be collected, and how those requirements change as needs become more complex.  This then connects to the previous post, where we see how different stakeholders influence this process.
# A Basic Machine and the Data it Generates

## The Data Needed to Run a Basic Machine

To explore what data is needed to run a basic machine, let's start with a very simple machine that cleans dirty beakers.  The **Clean Beakers** machine is basically just a conveyor belt that holds beakers and move them under some cleaning nozzles that spray cleaning fluids.  Let's imagine this machine operates in factory that mixes chemicals in glass beakers, and the beakers are reused.  The overall process of the factory can be seen in the diagram below.

<div class="mermaid">
flowchart TB
LB[Load Beakers] --> CB[Clean  Beakers] -->
MC[Mix Chemical in Beaker] -->
EB[Pour Chemical Out of Beakers] --> LB
style CB fill:#e69138
</div>

A person starts this process by loading beakers onto the conveyor belt.  The beakers are first cleaned, and then moved to another machine that mixes some chemicals in these beakers.  The mixed chemical is poured out, and then a person carries these dirty beakers back to the starting point.  This process could also be automated. So instead of people, a big conveyor belt moves the beakers between the four stations (each box in the diagram is a station) in this loop.    

Now that we understand how this beaker cleaning machine operates in the larger factory process, let's look at how the beakers are actually cleaned.   

<div class="mermaid">
flowchart TD
	BP[Beaker Arrives] --> BS[Beaker Washed With Soap] --> BW[Beaker Washed With Water] --> BC[Beaker Clean]
</div>

An upside down beaker (the mouth of the beaker is facing the floor) enters the machine on the conveyor belt.  The conveyor belt takes the beaker and moves it over the soap nozzle.  Once the soap is sprayed, the conveyor moves the beaker over the water nozzle and water is sprayed to rinse out the soap.  The beaker is now clean and is moved on to the next machine where the beaker is flipped over and the chemicals are mixed.

When this machine was being built and the requirements for what it should do were gathered, the engineering team(s) made decisions about what data this machine needed to run, and what data would be exposed to the outside world.  Typically, the more basic the machine, the more basic the data requirements might be. 

In the most minimal sense, what information or data would be needed for this cleaning machine to function?  Let's first define the way the machine works.
- The machine consists of a loading area, a single soaping area, and a single water rinse area.   
- There is a simple weight switch to detect if a beaker is present.  No weight means no beaker is present, and some weight means a beaker is there.  The switch doesn't actually measure the actual weight, it's just an on/off or yes beaker/no beaker.
- Below the soap and wash areas, there is a nozzle.  Each nozzle is connected to a valve that controls the water or soap being sprayed.  The valve is either on or off, there is no in-between settings.
- There are a total of 3 switches.  One switch where the beaker is loaded on the conveyor belt to detect when something is placed on the belt. One to detect when a beaker is above the soap nozzle. One to detect a beaker for the water nozzle.
- A simple motor moves the conveyor belt one step (e.g. from above the water nozzle to above the soap nozzle) at a time.  Each step is of equal length.
- A very basic industrial computer (e.g. a PLC) that controls the machine based on inputs from the switches.  The computer also controls the valves and the motor.

Based on that, this is the logic of how the machine functions and how the computer controls it.  The square boxes are actions performed by the machine/computer.  The ovals and floating text just provide more information on what is going on or the decision.



<div class="mermaid">
flowchart TB
    BP([Person Places Beaker On Conveyor]) -->
	LAS[Check Switch in Loading Area] 
	SS[Check Soap Nozzle Switch]
	SN[Spray Soap]
	WS[Check Wash Nozzle Switch]
	WN[Spray Water]
	LAS --> |No Beaker| DN[Do Nothing]
	LAS --> |Beaker Present| MC[Move Conveyor One Step]
	MC --> SS 
	SS --> |No Beaker| DN2[Do Nothing]
	SS --> |Beaker Present| SN --> MC2[Move Conveyor One Step]
	MC2 --> WS
	WS --> |No Beaker| DN3[Do Nothing]
	WS --> |Beaker Present| WN --> MC3[Move Conveyor One Step]
	MC3 --> BCF([Beaker Cleaning Finished]) --> |Repeat Process| BP
</div>

Another way to read this chart is to see what signals the computer receives and the decision made based on that signal.  For example, let's think about the "Check Switch in Loading Area" box.  If somebody places a beaker there, the switch gets triggered and the "on" signal is sent to the computer.  The computer then sends a signal to the conveyor motor to move it.  Instead, if that switch was "off" because no beaker was there, the computer would do nothing.  So the data sent to and from the computer only consists of different on and off signals.

If we were to record every event this machine does in order of what is done, it would look like this. This type of data is called **Event** data and is typically stored in an **Event Log**.

| Event | Time (hour:minute.second AM/PM) |
| ---- | ---- |
| Switch On: Beaker Placed on Machine | 1:00.00 PM |
| Conveyor Moved | 1:00.30 PM |
| Switch On: Beaker Over Soap Nozzle | 1:01.00 PM |
| Soap Nozzle Activated | 1:01.30 PM |
| Conveyor Moved | 1:02.30 PM |
| Switch On: Beaker Over Water Nozzle | 1:03.00 PM |
| Water Nozzle Activated | 1:03.30 PM |
| Conveyor Moved | 1:04.30 PM |

The steps above would be repeated over and over in the Event Log.

The computer might have some additional logic to make sure a switch is turned off after it's been turned on and other logic to handle possible errors.  But this is the basic logic required for this machine to run, and you can see the data we would get.

Keep in mind that this data might not be available to anybody to take off the machine.  The data you see above could be limited to the computer and no programming or storage was put into place for anybody to actually access this data.  So you shouldn't assume that if you plug your laptop into the machine computer that you'd be able to see this data.  Somebody has to write the software code to expose this data to the outside world.  But for the sake of making this easier, let's assume this machine stores this data somewhere and people can access it.

# Diagnosing an Issue Using Data

Now that we've described this basic machine, let's move to the scenario of diagnosing an issue using data. One day somebody notices the beakers are not being cleaned properly.  Let's also say that due to the physical design of the machine, it's hard to just look into it to see why the cleaning is not being done properly.  So nobody can just watch it running and see the issue.

If somebody pulls 10 days of log data (described in the previous section) to investigate, all of it is going to look exactly the same.  There's no clear indicator of the machine running differently, when the issue started, or what the cause is.  The log data tells us the machine is running and we can calculate things like uptime/downtime and utilization (how much this machine is used in a day), but we have very little information about what's really happening during the various processes the machine performs.

If you're only looking at the data, you're not going to be able to diagnose the issue, and you're certainly not going to be able to develop a predictive rule or model about when the machine is going to stop cleaning properly.  Despite the fact that the machine is running and providing data, the data isn't very useful.

If we wanted to diagnose the cause of the cleaning issue based purely on data, we could identify possible failure modes and the data needed to detect those.  Here are some examples.

| Fault Mode | Data Required to Diagnose |
| ---- | ---- |
| Nozzle Not Fully Opening | We need a way to track exactly how much the water and soap nozzle(s) opens |
| Water/Soap Pressure Issue | We can track the water pressure in the pipes that go to the nozzles, maybe at different locations along the pipes |
| Conveyor Movement Issues | Tracking how much the conveyor actually moves at each step might identify if the beaker is not placed at the right position over the nozzle |

If we were also collecting this data, maybe we'd see that the water pressure exiting the nozzle has gone down over time.  If we have multiple pressure readings along the pipes, maybe we could further diagnose it as a supply issue (not enough water is getting to the machine), a blockage issue (the water supply is fine, but one of the pipes in the machine is getting clogged) or a nozzle pressure issue (the nozzle is not spraying at enough pressure).  Based on this, we could build a simple model that Alerts on when the water pressure is trending downwards and is predicted to be too low (to clean the beaker effectively) in the future.  

This type of data is also known as **Sensor** data.  In contrast to Event data, sensor data gives you information about what was going on during a process, not just after it.  Event Data typically tells you about events after the event has already started or occurred, but has limited information about the conditions during the actual process that is occurring during that event.  It's the combination of Event and Sensor data that tells you when a process starts (a Start Event), what is going on during the process (the Sensor Data), and when a process ends (an End Event). 
# How do people decide what data needs to be collected?

Imagine when this beaker cleaning machine was designed, the original requirement was to build something priced to sell to small, low-volume factories with limited money.  Engineering and Product management decided the way to reduce costs would be to build the minimal product described above.

Over time, the machine became more popular and higher volume customers started buying it.  They also started complaining about it being difficult to service because it was difficult to diagnose issues.  These customers also started to make warranty claims out of frustration, stating the machine was malfunctioning due to product defects.  And since diagnosis was difficult, it was not clear if this was a genuine warranty claim, a maintenance issue, or people not operating the machine properly.

To fix this situation, the service organization proposed a solution. They would create a subscription service plan and fix any machine issues as part of the subscription cost.  To do that, they would need the machine to collect the data described in the previous section.  They also proposed a "subscription plus" plan which offered remote diagnosis of the problem.  That way the problem would be identified quickly before anybody even went onsite, and the service technician would already have the parts needed (along with a repair plan) to fix the machine as quickly and efficiently as possible.  To implement the remote diagnostic capability, the machine control computer needed to be upgraded (read: more expensive), the software needed to be updated to expose the data, and a software tool needed to be implemented to transmit data to the remote service center.

From a business standpoint, there are a few considerations with this subscription plan idea.
- The Service Org needs to provide a business plan demonstrating this subscription service is profitable, and it's worth the time, effort, and money for Engineering to update the machine
- Product Management needs to understand if customers are willing to pay for this updated machine and associated service plan.  Are there enough potential customers?
- Engineering needs to look at their priorities and decide if it makes sense to make these updates to the machine, or to have 2 versions of this machine.  One basic version, and one with the extra data capabilities.  And based on that, can they can support both machines?
- The Data Infrastructure org also needs to be made aware that this service is coming and they will need to build a pipeline to ingest the machine data, process it, and expose it in a usable way (e.g. Dashboards) to the personnel in the Service Org.  The specifications of the pipeline might influence what data is sent from the machine since the Data Engineers might need something specific that's not in the existing machine data requirements.  And whatever this costs to do, it needs to be included in calculations for the service plan. 
- The Marketing and Sales Orgs will probably need to be involved as well since they can help provide insight into the customer demand for these features.  Assuming the product updates are made, these orgs will definitely need support with their go to market strategy to market and sell it. 

# Moving to Predictive Analytics

Two years have gone by.  Product management and Engineering decided on two machines, and the second one not only included the extra data capabilities, but it could clean more bottles per hour.  This brought in a lot of new customers who were willing to pay for the plus plan that includes remote diagnosis.  Customers really like the new service, and the subscription plan is profitable as well.

However, there are two growing concerns.  The first is that the service department only knows about a machine issue when a customer calls and creates a ticket.  Basically, the service is 100% reactive and customers are already experiencing an issue by the time they call.  Customers are increasingly asking for automatic and proactive identification of machine issues before it affects them.

The second is that the remote diagnosis is completely manual.  After a customer calls, somebody with diagnostic training opens dashboards, identifies the issue, and then provides a recommend plan to fix it.  Since the diagnostic process is manual, the number of technicians has grown in proportion to the number of customers.  However, from a budget perspective, this isn't scalable because the number of diagnosticians required is cutting into the profit from the subscription revenue. 

To address these two problems, the Data org is asked to come in and evaluate the feasibility of  automated detection of machine malfunctions before the if effects the customer, sending out a notification Alarm based on this, and including a recommended fix plan with every Alarm.  The Data team spends some time evaluating this and provides the following recommendations
- Currently, the data from each machine is a daily summary of the sensor values.  For example, daily maximum water pressure, daily minimum water pressure, and daily average water pressure.  Since it is a daily summary, it is only sent once a day.  The Data team feels that for good predictive service, they prefer the raw un-aggregated sensor data in an ideal case, but hourly summaries would be sufficient.  It would also need to be sent 6 times a day.
- They need a budget for the infrastructure to develop and deploy these models
	- The infrastructure will need to integrate with the other systems, such as the Service system that manages customer tickets.
- They need a dedicated team of at least one Data Engineer and one Data Scientist to be able to support building the models, but also to support any tickets about the data quality, models, and Alarms.

All of the above will eat into the revenue pie of the subscription services, so it's important to estimate how much automating these things will save the company compared to just hiring more people to perform the remote diagnoses.  In other words, how much cost reduction is realistic if the Data org is able to successfully implement their ideas?  How accurate do the Alarms and recommendations have to be to actually realize these cost savings and for it to be considered a success?
# The Motivations for Data Collection

At this point we can stop building out this story.  There are many different directions this could go, all of which would further illustrate the original goal of this post and the previous one; that data is not collected and provided just because that's what one does.  Collecting data takes time, effort, and money, and there needs to be a useful reason to why it's done.

Stakeholders can be very differently oriented in how they think about the value of data and where investments should be made.  Engineering is focused on what data is needed to ensure the machine runs.  Service wants data that allows them to service the machines as cheaply and quickly as possible while balancing driving down service costs with keeping customers happy.  

In the predictive analytics section of this story story, the Data team is really an extension of Service. If the Data team said they wanted higher frequency data just because they want more data, Engineering would completely deprioritize the request.  However, since this is really a request to reduce costs and improve what the Service org provides to the customer, the Engineering group might be pushed to prioritize it.

Sometimes these relationships are adversarial.  The Engineering org could disagree with the way the Service org wants to do this and they feel the machines can have the predictive algorithms running on the machine computer.  That way there's no need for the Data org to get involved at all, and the Service department doesn't need to own it.  If the code is running on the machine, the slice of the subscription revenue allocated to the predictive service feature will go to Engineering and not the Service Org.

If we think back to the previous post where we talked about all of these functions being in the same company versus multiple companies, each one has it's positives and negatives.  For example, if the company that built this machine was purely a machine builder, they might say they don't want to get into the servicing business.  The most they'll do is some minimal work to make it easier for service providers to add data export and transmission capabilities to the machine.  When the machine builder and service are different companies, there's a clearer delineation on where each should focus.  If the Engineering and Service org are in the same company, there might be more push from upper management for the groups to work together, since any cost reduction will benefit the company as a whole.  At the same time, this might result in a worse quality predictive service product because both of them have no choice but to work together and neither wants to hold the other accountable for issues and risk harming existing relationships.

# The Evolution of Industrial Machines and Technology

In the example in this post, the machine is very simple and only needs a very basic computer running a relatively simple set of instructions to control the machine.  Even if the machine was collecting the hypothetical sensor data, it would still only be a few sensors and an easily manageable set of data.  

In contrast, there are machines so complex they have hundreds or thousands of sensors, multiple computers, and lots of additional microprocessors to control everything.  The software running on these machines is also just as complex, and is basically a product in itself. Examples of these complex machines include the lab equipment used to analyze human blood samples and machines used to manufacture microchips.

When you hear about or see these types of machines, it's hard to wrap your head around how it was designed and built.  One question I always ask myself is how did people even start or plan it out?  Imagine if somebody asked you to build a car.  Where would you even start?  How do you build something so complex from scratch?

The answer is, you don't build it from scratch.  The complex machines are based on the lessons learned from less complex machines, which are based on lessons learned from machines that were less complex than those ones.  Over time, people make mistakes and learn how to make things better.  They also learn what is needed in the real-world and they evolve their products.  Lots of new machines have hardware and software in them that was made and used in an older machines.

As an example of this, let's look at the computer that controls these machines.  At first, the control systems were basically all hard-wired and it was very difficult to change the logic.  Then came the [PLC](https://en.wikipedia.org/wiki/Programmable_logic_controller) which gave people much more flexibility than hard-wiring the logic to control the machine.  Modern PLCs are basically regular computers (with lots of cores) that have all the software and hardware (e.g. connectivity) to operate in an industrial environment.  They can also be extended to run docker (e.g. python in a container), a messaging bus, a relational database, you can attach an NPU (for Deep Learning Inference), and do lots of other things.

With this machine evolution comes an evolution in what data is collected as well.  As machines are designed and used by customers, people realize what additional data needs to be collected in the next version of the software or machine.  Over time, as sensors become smaller/cheaper, and programmable microcontrollers become more flexible and powerful, having more data becomes essential to these more complex machines operating properly.  Then the discussion changes from "why collect data" to "what's the right data to collect?"  Here are two examples of this.
- A complex machine already had lots of different event and sensor data being used to control the machine, but due to the high data volume, most of it was not stored and thus never exported.  In fact, the software and database on the machine weren't really designed to store and export all that data outside of some temporary debugging modes.  To enable any additional data export would require a major software update that didn't cause issues with any existing data exports.  So for any additional data exports, there had to be good rationale for what data was really needed to address any use cases.
- A factory was collecting data from multiple machines that were all part of the manufacture of a single thing.  A 6 month sample of this data (thousands of sensors) was multiple terabytes compressed.  There was a very expensive issue that was occurring, and they were trying to better understand the circumstances under which this issue occurred, **not** build an accurate model to predict the issue.  After an extended timespan where multiple internal and external teams of specialized Engineers and Data Scientists looked at this, the conclusion was that despite collecting so much data, they were not collecting the right data.  
 
Note that data limitations do not arise only from machine data.  Certain use cases require the alignment of machine and non-machine data and this is not always feasible.  One widget manufacturer was facing the problem that some widgets would crack during the final stages of the manufacturing process, and there was a theory that it was a combination of manufacturing process and some supplier quality issue.  There was some issue with the raw material that resulted in a reaction to some condition in the manufacturing process.  All raw material did undergo a quality review when it was delivered, but nothing stood out.  In this particular factory, each widget was created individually, but in later stages many widgets were batched together (e.g. heating lots of widgets together in an oven).  So there was no way to match or align the sensor or quality data with the individual widgets and match the data to the widgets that cracked.

With what I've just said, I don't mean to imply that machines and data collection have reached this point where every machine is generating a lot of data and the challenges are now around data alignment/cleaning and figuring out which is the right data to collect.  Many machines collect minimal data that is useful only for some basic KPIs.  It also takes time for Industrial customers to upgrade and update their machines.  If a factory was built in 1993, it's almost a given that many of the original machines are still operating 20+ years later.  And those machines were were probably designed based on technology older than 1993!  Unless there was good reason to upgrade,  the computer controlling those machines would still be that old.  That's assuming the computer controlling the machine isn't a human!

# Wrap Up

I hope this post gives some concreteness to the idea of a machine and what data it generates.  I realize the topic will still feel a bit vague since I've given general examples of machine data, but there's no spreadsheet here with data extracts from machine and non-machine data.  My intent was to add clarity to the idea that data isn't collected for the sake of being collected, and that there are conscious decisions made about what data is collected an exposed.  Also, the calculus around these decisions changes as technology improves, and the justification needed to make a decision today might be different than what was needed a decade ago.   










