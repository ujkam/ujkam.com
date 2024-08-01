+++
title = 'Calculating Downtime is Harder Than it Sounds'
date = 2024-07-31T00:00:00-04:00
draft = false
mermaid = false
+++

Predictive Maintenance is fundamentally about two goals - keeping your machines running, and making them run in the best way possible.  In other words, focus on reducing downtime and continuously optimizing how your machines run.  The concept of downtime is easy to describe and understand, but calculating it can be much more complex than people realize.  In this post I'll walk through calculating downtime for a factory machine, and how the complexity of the calculation reveals why using predictive models for predictive maintenance is so challenging.

<!--more-->

# Introducing Downtime

Before discussing anything else, I'd like to make my goal for writing this post explicit. I'm not really trying to teach anybody how to calculate downtime. What I'm really trying to do is illustrate there being a lot of complexity and uncertainty in calculating downtime, a fundamental metric to understanding how well a machine runs.  And if something so simple sounding can be complex, it hints at how complex other KPIs or metrics can be.  This has a direct impact on how difficult it can be to create a dataset to train a useful predictive model to address a predictive maintenance use case.

To provide some context, let's first describe a concrete scenario. Imagine we have a factory which manufactures something common and tangible.  Examples of this include plastic bottles, napkins, an electrical switch, etc.  Feel feel to pick whatever you like.  In a broad sense, there are three major activities at the factory.
1. Receiving, inspecting, and testing raw materials
2. Using machines and people to turn the raw materials into the product we are manufacturing
3. Final Inspections, packaging, and shipping of the manufactured product to customers

We are mostly going to focus on activity 2.

Let's start by broadly defining **downtime**, and we will evolve this definition later. **Downtime** is the calculation of how much time a machine is not running.  The opposite of **downtime** is **uptime**, and you can usually calculate one from the other.  For example, if a machine is stopped for 4 hours in a 12 hour period, the downtime is 4 hours and the uptime is 8 hours. 

To calculate downtime for one of our machines, what data would we need?  We can start by looking at the machine log data (this is collected by the machine as it runs), which *hopefully* contains all the times it started and stopped.  In the real world, many machines do not log this information, or they only log selective start/stop events.  But for the sake of this story, let's assume our machine collects and logs these events correctly.

We can calculate downtime from start and stop events by using logic such as the following.
- If we see a start event at 11AM and a stop at 12PM, the machine ran for 60 minutes.
- If we see a stop event at 12PM and a start event at 1PM, the machine was stopped for 60 minutes.

By simply adding up these time spans, you can determine how much time the machine was not running over the span of a day, week, month, etc.  The sum total of these numbers gives you a very rudimentary downtime calculation. 

Now let's add the first wrinkle in this calculation.  The machine logs shows the machine stopped at 4:30PM on Friday, and started at 8:00AM on Monday. Machine logs won't provide a reason for this, but we can guess it.  To properly calculate downtime, we need to incorporate the company calendar to account for weekends and holidays.  If we are calculating downtime for different sites and geographical locations, adding in shift information may be useful as well.

Adding this information changes our conceptualization of downtime.  What we really need to know is **when the machine was not running when it was supposed to be running**, and not just assume a machine not running equals downtime. What we need to do is split our downtime calculation into **planned** and **unplanned** downtime.  

# Planned and Unplanned Downtime

**Planned** downtime is when a machine is stopped and the machine wasn't supposed to be running.  If we were performing a weekly downtime calculation and had included the two day weekend as downtime, we would conclude the machine was down for 28% of the time, even if it ran perfectly during for five working days.  Including planned downtime in our overall downtime calculation will overstate the downtime, so we shouldn't include it.

**Unplanned** downtime is when the machine was not running when it should have been running.  In my experience, this is what companies really want to know.  The goal is to reduce unplanned downtime as much as possible.

## Planned Downtime

Beyond weekends and holidays, planned downtime includes things like quarterly scheduled maintenance, rebuilds, and all the regular maintenance activities occurring on a known cadence. 

Planned downtime also includes events that are little more squishy, when it's harder to track exactly when the machine started and stopped.  This includes stoppages when materials need to be replenished, when the person operating the machine changes, some quick maintenance (e.g. adding some fluid), and something called changeover time.  Changeover time occurs when you do something like changing a die/mold in a machine.  If these events aren't included in any data sources, assumptions needs to be made about if these stops are planned or unplanned.  I'll discuss this more later.  

## Elaborating our Definition of Downtime 

We can update our definition of downtime now.  **Downtime** is a calculation of how much time a machine is not running when it should be running.  In more formal terms, **Downtime** is how much **unplanned** time the machine has not been running.  At least in theory **Downtime** can also be calculated by taking the total time the machine was not running and subtracting the **planned downtime**.

Based on what we've stated already, the data sources needed to calculate Downtime are:
- Machine Data Logs for Start/Stop times
- Company Calendars (Oracle/SAP/Spreadsheets/etc.  Hopefully not paper)
- Shift Schedules (ERP and other systems.  Hopefully not paper)
- A source containing "Squishy" stops.  For example, the Machine Data Logs may record when the machine operator changed (e.g. a new operator logs into the machine), the machine was stopped for a die change, etc.  This might be in a different log file than the start/stop log.  Some reconciliation needs to done between the different log files to correctly calculate the stop times.

Using the data listed above, we can figure out the total downtime (using the machine data logs) and the planned downtime (using the other data sources).  We then get the total unplanned downtime by subtracting the latter from the former.

Before moving on, let's pause here to talk about the realities of calculating planned downtime.  All this data exists in different sources and need to be reconciled.  If all your machines exist in different time zones and geographies with different working laws, standards, and holidays, your data reconciliation needs to account for all of this.

Reconciling the squishy parts is challenging since you can't always directly track what event really happened.  For example, if your machine records a stop and then 10 minutes later a new operator logs in, was this a shift change?  Why did it take 10 minutes?  Was there a coffee break?  Or was there an issue with the machine the first operator could not figure out, so they called a more experienced person to try an diagnose it?  What if the operator/personnel changes aren't recorded in the machine logs at all, but according to the shift schedule the operator should have changed.  Why did it take 10 minutes?  Was it a planned or unplanned stop?  Is 3 minutes a better time for an operator change?  Maybe 15?  

In many cases, the only way to handle this is to just set a time limit for when something should be included in unplanned downtime.  For example, any stoppage for less than 5 minutes is considered as planned downtime if the cause is not known. 5 minutes or more is considered unplanned downtime if we cannot clearly identify it as planned downtime. Somebody needs to make a decision about the limit, and these types of squishy calculations always end up influencing downtime calculations. I'll discuss this in more detail later.

## Unplanned Downtime

Now that we've talked about the stoppages we want to exclude from our downtime calculations, let's discuss the stoppages we want to include (unplanned downtime).  From a business and operations perspective, these are the stoppages we want to reduce or eliminate so our factory runs more efficiently.

Let's extend our story.  We've taken our machine data, determined the stoppages, and removed everything overlapping with weekends, holidays, recurring maintenance periods, and all other known planned activities. The time remaining is unplanned downtime.  Now the question is, is this good enough?  Do we take this number and calculate it as a fraction of the total target runtime (e.g. 8 AM - 4PM everyday) and build our downtime KPI?

Typically KPI building is not just an exercise in quantifying something.  In a manufacturing setting, one of the reasons to have a KPI is so you can track and improve it.  And to improve a KPI, you need to understand what is driving it.  For downtime, you need to be able to identify the root cause of it so you know where to focus your remediation efforts.  For example, if 70% of your downtime is caused by a single part breaking over and over again, you should focus on figuring out how to reduce or prevent it.

Determining these root causes forces us to both broaden and deepen the data we are using.  To broaden our understanding of the environment in which this downtime is occurring, we need to look at more data sources.  This enables us to dig deeper into our data to see what specifically is going on during a downtime.  To make this point more clear, let's look at some examples of the root causes of unplanned downtime.  

### Machines Do Not Exist In Isolation

Machines can stop because of issues with the machines around them.  If there are 10 machines in a row and the first machine feeds the second, the second feeds the third, etc., any issue with one of these machines will result in the other machines stopping.  For example, if the second machine has an issue, all the machines after it will be "starved" because they are not getting the supply of what they need from the previous machine.  This works the opposite way as well.  If machine nine is having issues and cannot "consume" what is coming out of machine eight, every machine before nine has to stop.

In this scenario it's important to realize you have to classify these unplanned downtimes differently.  Out of those ten machines, only one machine is really down.  The other machines are not really down, they are available and ready to run, but they are simply stopped.  The machine with issues it not **Available** to run, but the other nine machines are **Available** to run.  This distinction is important because you want to understand why your machine stopped, not just how long it stopped.  

This idea of **Availability** is defined as part of a metric called **OEE (Overall Equipment Effectiveness)**.  OEE tries to provide a more robust metric of your manufacturing productivity in comparison to just looking at downtime or output.  I'll discuss OEE in more detail soon. 

If we are calculating a downtime KPI for the entire factory, saying the factory is having unplanned downtime because a single machine has blocked all production is an accurate statement.  But at a machine level, saying all ten machines are having unplanned downtime is misleading.  It presents an inaccurate picture of what is really going on.  Correctly attributing what is going on with each machine allows us to correctly identify the root cause.

### Drilling Down into Subsystems

Until this point, we've mostly focused on downtime at a machine level, or looking at the downtime for an entire machine.  From a root cause perspective, this an overly simplistic view.  Some machines can be very complex and are better envisioned as multiple machines working together as one big machine.  An analogy to this is a car. A car is a single machine, but it's really a bunch of complex machines (e.g. the engine, the transmission, the heating/cooling system) working together.  A modern car has multiple computers controlling each system, and the computers talk to each other, ensuring the car functions transparently as a whole.  Complex machines in manufacturing work the same way. 

When we establish the root cause of downtime, our goal is to identify the specific part of the machine causing the downtime.  In a complex machine, we're really asking, "what part of which subsystem caused the downtime?"

This type of root cause attribution is typically done in two ways.  First we can look at the machine data for clues.  Machine event logs should (hopefully!) contain events occurring before and after the stops.  Some machines track a lot of events, and they may log a "reason" for the machine stoppage. Note this process is less trustworthy than it sounds.  Sometimes the reason logged for the machine stop is not the cause of the issue. 

To provide a historical example of an incorrect cause and effect, we can look at the time when smart phones started to have built-in GPS's and people started using their phone navigation systems.  Typically, the phone was placed on a phone holder on/above the dashboard, and was directly exposed to the sun.  At the time, using the GPS system on the phone caused the phone to heat up considerably.  On a hot sunny day, the combination of GPS usage plus direct sun (through the windshield) would result in the phone overheating and shutting down. If you were to look at the sensor and event logs of the phone, you'd probably see the GPS being used, the temperature steadily climbing to a limit, and then a shutdown event.  Just looking at the data, the "cause" is the GPS.  In reality, it's really the direct summer sunlight and the design of the GPS system.

To add to the challenge of determining which subsystem is having issues, sometimes the data does not clearly identify when a single subsystem stopped.  What you might have to do is infer the stop based on other machine activities.  For example, if a routing system (e.g. a conveyor belt)  log shows your machine stopped sending widgets to a particular subsystem, you have a sign something is going on.  But is the subsystem down?  Is there something broken with the routing system?  Is a downstream subsystem having issues, blocking this subsystem? These are all possibilities, adding to the complexity of attributing downtime correctly.

### Throughput Issues

The last issue I want to discuss is throughput or reduction in production output.  If your machine is able to produce 100 widgets per hour, but there is an issue reducing this to 50 an hour, there is clearly a reduction in **performance**.  **Performance** is a metric used in OEE.  

If the machine output is lower than expected, this isn't considered downtime for the overall machine.  However, it's possible one of the subsystems stopped working and is the reason for the production loss.  That subsystem should be counted as having downtime.

One scenario where a subsystem can be down, but the machine is still running, occurs when a machine has redundant systems doing identical work.  Let's say a machine has four identical subsystems to clean bottles.  When a bottle arrives to be cleaned, it's routed to the subsystem which is available to receive it.  When one of the subsystems stops working, there are three others working.  Redundant systems offer the dual benefit of increasing throughput by doing work in parallel, and they also help ensure your manufacturing line isn't completely stopped because you only have one subsystem for a particular task.

Throughput loss is just another example of the complexity you see in the real-world, making downtime calculations less straightforward than it initially seems.

## Redefining Downtime

With all this new information, we should revisit our downtime definition.

**Downtime** is a calculation of how much time a machine is not running when it should be running.  In more formal terms, **Downtime** is how much **unplanned** time the machine has not been running.  At least in theory **Downtime** can also be calculated by taking the total time the machine was not running and subtracting the **planned downtime**. It's also important to correctly identify when a machine was down because it was having an issue, or if it was **available** but stopped. 

The Data Sources needed to calculate Downtime are:
- Machine Data Logs for Start/Stop times for all machines so we can identify which machines are truly down, and which are available but blocked due to other factors.
- Other Machine Data sources, including operator logs, subsystem logs, and sensor data, allowing for downtime tracking on subsystems, and also for root cause identification.
- Company Calendars (Oracle/SAP/Spreadsheets/etc.  Hopefully not Paper)
- Shift Schedules (ERP and Other Systems.  Hopefully not paper)
- A source containing "Squishy" stops.  For example, the Machine Data Logs may record when the machine operator changed (e.g. a new operator logs into the machine), the machine was stopped for a die change, etc.  This might be in a different log file than the start/stop log.  Some reconciliation needs to done between the different log files to correctly calculate the stop times.
- Service Data to see what repair/maintenance work was done.  This data can help identify the root cause of issues

# Squishy Stops

I've talked a bit about "squishy" stops but haven't really provided many details. Realistically, you can't clearly identify the cause of every stop.  The reason I called it "squishy" is because it's not clear if a stop falls into the planned or unplanned bucket and there's limited data to strongly support either case.  What happens is you end up in a situation where you have short stops of various lengths (e.g. 1 minute, 3 minutes, 7 minutes, 10 minutes). You might feel like ignoring or discarding these, but they can add up to a lot of time, which can greatly change your downtime KPI.  This forces you to include them in your KPI calculations.

A typical way to deal with squishy stops is to have a cutoff based on the expertise of the manufacturing and operations people.  For example, they might know most of the quick maintenance activities (e.g. refilling a fluid) can be done in under 5 minutes and propose that as a cutoff.  Anything shorter than 5 minutes is considered planned (and should not be included in the downtime KPI), and anything longer than 5 minutes should be considered unplanned downtime unless it can be clearly attributed otherwise.  

One challenge with using a cutoff is moving this value by a seemingly insignificant amount (e.g. 5 minutes to 6 minutes) can result in a meaningful change in the downtime KPI.  This change could be large enough to move a conversation from "KPI looks okay but could be better" to "This is a significant problem and we need to escalate."  When the KPI is built, it's important you trust the experts picked that cutoff value for the right reasons, and not to game the KPI.

# OEE (Overall Equipment Effectiveness)

You might be reading this article and asking yourself how manufacturers deal with all of this and come up with useful KPIs.  Note the challenges of tracking downtime, and figuring out what is downtime and what is not, is well recognized in the industry.  There's an industry standard metric called OEE defining this formally and standardizing it.

OEE is a combination of three KPIs.  Availability, Performance, and Quality.  We've already talked a bit about Availability and Performance, but let me summarize.  Availability is measuring the percentage of time your machine is available when it should be available.  Availability is directly related to downtime.  Performance is not about downtime, but really about throughput.  It's a measure of how much you are producing in relation to some theoretical maximum.  

Quality is a measure of what percentage of what you produce is defective.  This metric isn't directly related to downtime or this article, but it does relate to predictive maintenance.

Something I like about OEE is there is flexibility in the how the calculations for the underlying KPIs are defined.  When calculating OEE, it's understood different machines and manufacturing facilities produce different data, and people can only define things like Availability and Performance based on the data they have.  So there's room to deal with the squishiness of the data behind these KPIs.

https://www.oee.com/ has some great information on OEE, including examples of how OEE is calculated, and also a scenario where OEE is used to identify the root causes of low OEE and how to address it.

# Data Quality and Predictive Maintenance 

Calculating downtime to a reasonable degree of accuracy requires us to reconcile all the data sources described in this article.  This reconciliation is much harder than many people realize.  Here are a few examples of why.
- Different machines are built by different machine builders (vendors), and the data collected from each machine can be very different from other machines.
	- Different machines record different "levels" of stops depending on how the machine was programmed.  One machine might only record hard stops, such as safety stops and other serious stops.  Some machines log 10 different kinds of idle modes and multiple types of stop signals, so a lot of logic needs to be coded to determine when the machine actually stopped running.
- The amount of data recorded from each machine is dependent on the technology available when the machine was built.  When a particular machine was designed in 2018, it was safe to assume customers' factories would have high speed networking and they would not be surprised if a machine generated a gigabyte (uncompressed) of data per week.  If the machine was designed in 2005, engineers would have a very different idea of customer's data networks and capabilities, meaning machines would collect and report much less data.
- Matching different datasets at a minute to minute level can be extremely difficult, and sometimes impossible if different datasets are reporting data at different frequencies.
- Operator Data, Service Data, Shift Schedules, etc. are all in different systems and created using different technologies and interface elements.  Old touchscreens (which people hate using), modern tablets (which are inconvenient for typing large amounts of text), laptops, drop downs, free text fields, UIs in a browser, a 15 year old application running on Windows 7, etc.  Paper is also more ubiquitous than people realize, and it usually ends up being manually transcribed into in Excel.
- Companies have very different tolerances for how much data to record and store. This has greater implications to predictive models than calculating downtime, but it means sometimes data simply isn't saved in a way allowing people to use it for analytics 

I've got two anecdotes to add some concreteness to the above.

I was asked by a manufacturing operations team to help "predict downtime" which they didn't clearly define.  When they sent me data extracts from the machine, there were large gaps.  The machine log would show a stop event, followed by a large gap of time with no data, and then there was data showing the machine was up and running without any signs of starting back up.  The opposite happened as well.  The machine would be running, there would be a gap in the data, and then it would be running again, only with signs there was a stop and restart.  It wasn't clear to anybody if this missing data was not being recorded, if there was a bug in the logging, if the network was failing and data was getting lost, or what was really going on.  The only stops clearly being recorded were safety or sudden manual stops performed by a person.  The only way they could fix this would be to call an industrial engineer to check and possibly update the software on the machine, which they weren't willing to do because the machine was very old.  Nobody wanted to take a chance of breaking the machine software.

In another case, there was a complex machine (with many subsystems) which generated lots of different logs at different levels of event granularity.  The most detailed log generated about 3 Terabytes of data a month.  This was for a single machine, and they had 500+ of them globally.  Using this detailed log would give them a much more robust way to understand downtime, but they simply didn't see the business need to be so precise in their KPI.  That data might have enabled other predictive maintenance use cases, but the business justification just wasn't there to collect and store it.

# Wrap Up

Let me go back to what I said at the start of this article.  Predictive Maintenance is fundamentally about reducing downtime and optimizing how a machine works. A model may be predicting the failure of a particular part, detecting abnormal operations, or trying to optimize the quality of something produced.  At the end of the day, downtime and/or a machine running sub-optimally means you produce less, it costs you more to produce whatever you are producing, which reduces your profits.  Every predictive maintenance model is simply a small way of tackling one of these two problems.

Statistics, Machine Learning, and Deep Learning (AI) exist because many signals or trends cannot be detected by simple rules or looking at charts.  We use these algorithms when we know the data is noisy and the logic to separate the signal from the noise is complex.  To build a good model, you need good data.  And to have good data for the model to learn from, a person needs to clearly identify when an issue has occurred or not occurred so an algorithmic model can learn to detect or predict this.

What I've tried to show in this article is how challenging it can be to even calculate something as simple sounding as downtime.  If something so basic and essential has a "squishy" component, try to imagine how complex it is to build a well labeled dataset for a predictive model.  And there are "squishy" components in building these training datasets as well, which means if you change something as simple as a cutoff, the effect on the model can be very dramatic.  I talked about this in detail in my [last article]({{< ref "False Alarms can Erase the Value of your Predictive Maintenance Efforts" >}}) about the effects of label noise on model performance.

I realize using the challenges in calculating downtime as evidence to support the challenges with building a model training dataset is quite a leap in logic.  But I hope this article gives people a glimpse into why so many predictive maintenance models flounder and struggle due to data issues, and why data and data quality investments are worth it.