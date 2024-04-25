+++
title = 'The Anatomy of a Predictive Maintenance Project'
date = 2024-04-15T00:00:00-04:00
draft = false
mermaid = false
+++

When conceptualizing and implementing a predictive maintenance project, it can be hard to grasp the entire chain of people and technology needed for success.  In this post I'll try to break down all the people and teams needed for this type of project, and also provide insight into how these people work together.

<!--more-->

An alternate title I had for this post was "How much is my predictive maintenance program really going to cost?"  The goal is not give a dollar amount, but really to understand how all the different people and pieces fit together so you can fully conceptualize the scope of one of these programs.

Lets start with the assumptions.  This post is focused on projects that actually make it to "production", which means they generate some real financial benefit. R&D work, proof-of-concepts, exploratory activities, etc. are all valuable, but they have working patterns that can vary quite a bit from what I'm describing here.

This post focuses on predictive maintenance projects addressing specific use cases, not general purpose solutions.  A general purpose solution is something like an anomaly detection SaaS trained on a variety of machine data, but it needs to be customized for each use case.  Developing and using this type of SaaS is different from what this post is addressing.  I'll provide an example of what I mean by "specific use case" soon.

I've also pointed at a "Data Scientist" as the person who implements these projects, but more realistically it could be any person with data and software skills.  Job titles mean different things at different companies and there nothing stopping a Data Analyst or Data Engineer from doing these things.  So don't take my using the words "Data Scientist" as some sort of gold standard for who needs to do this work.

For the sake of simplicity, let's split these projects into two types, **low** and **high** complexity projects. "Complexity" refers to the number of people and the the computing environment used to develop and deploy a model, and also to evaluate and act on **Alarms** (the model inference/predictions).  Complexity is not referring to complexity of the actual model.

A **Low** complexity project consists of a smaller group of people working on a focused use case, with a more manual process around evaluating and acting on alarms.  A **high** complexity project has a lot more people and technology in the development of use cases and evaluating and acting on alarms.  Again, it's not about the actual model, but everything around the model.
# A Low Complexity Project

Let's start with an example.  A Subject Matter Expert (**SME**) identifies a machine issue (a failure mode) they'd like to predict, and proposes rules that could be used to predict this.  They work with a Data Scientist to codify (e.g. using Python) and test those rules, and both of them work together to evaluate and adjust those rules to improve predictive performance.  Once they are both happy with the results, the Data Scientist deploys the "production" script into a system that runs it once a day.  After the script runs, it emails a list (also known as alarms) of all systems that violated those rules to the SME.  When the SME gets the email, they evaluate the alarms by investigating the machine sensor data.  For any systems with genuine issues, they manually create a service ticket for the relevant system so somebody can intervene. 

There are a few points I'd like to highlight.
- The "deployment" system could be as simple as a laptop running the script once a day using the operating system scheduler.  Granted, with cloud services everywhere, a laptop  is less likely than the cloud equivalent (e.g. a small cloud VM).
- The "production" script basically fetches some data (from a database, from a file), does something with this data, and sends an email.  It's not necessarily built with software engineering or DevOps best practices, logging, or even very tolerant of failure.
- The impact of the script failing is low.  If the laptop turns off and the script doesn't run for a few days, only the SME will notice and care.  So there limited need to harden the script and the deployment environment to prevent these types of issues.
- The "monitoring" is done by the SME, and they ensure bad alarms don't reach the service personnel.  If the SME goes on holiday for two weeks and nobody looks at the alarms, it doesn't result in a organizational or contractual issue.

The complexity of this project is low because the organizational overhead is low.  The entire process fits on a single slide, and the entire project is executed by two people.  Any support needed from other people is minor (e.g. somebody in an IT role needs to allow the script to send emails).
## Scaling This

One major challenge is this entire process being bottlenecked by these two people.  A few sick days means alarms are no longer checked or tracked.  If either of them have other job responsibilities taking up a lot of mental space, they may not remember, or have the time, to check the alarms or if the script is still running.

Once you get past a handful of use cases, it's difficult to sustain this low complexity model.  If nothing else, the Service organization is going to ask why SMEs are suddenly creating service tickets based on a "predictive model", and will also ask for a business justification for this new way of working.  The SME's management structure will need to step in.  When two different parts of an organization need to work together, a process will be need to be defined. 

Unless your goal is to stay small, trying to scale and resolve these types of issues is what changes this from a low complexity project to a high complexity one.  To scale, you'll need to bring in other people and other groups and find a way for them to work together.
# A High Complexity Project/Program

Here's an example reflecting a more complex situation.  A team of SMEs come up with failure modes they'd like to predict, and each of these failure modes is given a business value to help prioritize which to work on.  A team of Data people work with them to build resilient data pipelines and rules/models.  As part of the development process, every project is tested for a fixed period of time, and the quality of alarms are summarized using predefined metrics.  Only the models passing a defined target can then be moved to a standardized and supported production system.  To move the project to production, there are requirements the code has to meet for logging, input/output, and so it is resilient to upstream/downstream issues.  Once the project is in production, the alarms go directly to another business team who monitor and act on them.  The data, SMEs, and business teams are also all involved in monitoring alarm quality over time to catch any changes in behavior.

For the sake of clarity, let's define all the teams of people above.

**The SME Team:**  The fundamental goal of this team is to come up with ideas or use cases, and to provide support when the Data team builds solutions.  This is typically a cross-functional team and they may only do this part time (e.g. 90% of their job is something else). So from an organizational perspective, they report to different managers and have different overall goals for their jobs.  They may have mixed backgrounds and motivations and aren't always aligned with each other.  

For example, one member of this team could be from Customer Service, with a focus on issues directly affecting customers.  Another person could be from Engineering, and they are focused on hardware issues causing machine damage, even if the occurrence of these issues are low.  A third person might be very finance focused and interested in issues which cost the company the most money, regardless of customer impact.  Keep in mind a company can have multiple products, so you might have multiple SME teams competing to get their voices heard and use cases prioritized.

**The Data Team:**  This team will have three core functions. 1). Doing data things such as building data pipelines, analyzing it, building models, etc.  2). Building/Maintaining a development and production infrastructure. 3). Ensuring the outputs of the previous two items are useful to other teams.  This could include dashboards so alarms can quickly be checked and validated against other data (e.g. machine sensor data), an app to track and handle alarms, documentation, etc. 

Part of this is working with other teams who own the upstream systems (e.g. the raw machine data), and also the downstream systems (e.g. the service ticketing system) so all the production infrastructure can talk to these systems in an efficient and standardized way.  The infrastructure also needs to be resilient to failures, so any upstream and downstream issues do not result in the production system crashing and losing alarms, or worse, generating incorrect alarms.

**The Service and Alarm team:**  After the alarms are generated, someone has do something with them.  This could be a single specialized team, or an entire global organization with regional teams, different IT systems, and different operational procedures. The people on this team could be call center personnel, onsite technicians, service SMEs (e.g. technical experts), or managers who are looking at the business side of things.  This team defines what should be done with those alarms when they get them, and they define what they need from the other teams so they can take effective and prompt action.  The Service team also monitors the alarm quality in the short and long term to ensure they are not wasting time and money on bad alarms.

**Governing Team:**  This team is a necessary evil to ensure the teams above are aligned and all working towards the same goal.  I say necessary evil because a Governing team will slow things down in the short-term and make it harder to take fast decisions.  At the same time, a good Governing team will help ensure the project doesn't go in the wrong direction for eighteen months and then get shut down.  With so many cross-functional teams, it's almost inevitable people will focus on their own goals, even if those goals are contradictory to the overall project being successful.  This team defines standards and procedures to follow, and they enforce a quality bar ensuring everybody knows what the targets are.  

Unfortunately, you can also have a bad Governing team which creates misery in the short-term and failure in the longer-term.  So having a Governing team is not automatically a solution to cross-functional drama, and they can make things worse, resulting in a lot of passive-aggressive behavior and the inevitable failure of the project.

**Hidden Teams:**  There are also other teams involved in this process that aren't always at the forefront, but they are critical to it's success.  An obvious example are the people who maintain all the IT systems (e.g. Databases, ERP/Ticketing, Dashboard/BI).  They are the ones who built these systems in the first place, and they are the ones who fix issues and implement changes.  You also have Data Engineering teams, who build pipelines to ingest raw data and make is useable to other teams.  Software Vendors also hide behind all of this. A documentation team may also be needed to take what is created by these individual teams and expose it to the wider world.  And if you are selling predictive maintenance as a service, don't forget sales and marketing.
# How These People and Teams Work Together

## SMEs and the Data Team: 

Typically the people who spend the most time working together are SMEs and Data Scientists.  This is because these are the ones who conceptualize and build a solution.  At a bare minimum, SMEs have the domain knowledge to identify, support, and justify a predictive maintenance use case.  And a Data Scientist has the machine learning and software skills to figure out how to, and if it's possible, to build a solution they can apply to a fleet of machines.

Here's a example of how they work together.  An SME is tasked with reducing unplanned downtime on customer machines.  They look at the historical service data and identify the top 30 issues leading to the most downtime.  They combine this with their domain (e.g. knowledge of the the machine hardware) expertise to filter this list down to issues where it makes sense to try and proactively address them, and they suspect could be predicted using a model.  Based on some further business and financial criteria, these use cases are prioritized.

This prioritized set of use cases is typically where the Data Scientist starts.  They work directly with the SMEs, who provide all the knowledge about the use case, the machine, and the parts/components and how they are related to the failure to be predicted.  This can include information about the software logic controlling the machine and generating the machine data.  Working together, they also find examples in the historical data of what is supposed to predicted.  Using this information, the data person tries to build a model that can be applied to every machine in the field.  It's also the Data Scientist's responsibility to evaluate the predictive quality of their models, and validate these quality results with the SMEs.

**In my experience, what I've described in the last two paragraphs takes up the bulk of the time with any predictive maintenance project.**  Unfortunately, this is also the most unpredictable from a time perspective.  I've been involved in projects which went from first conversation to production in 5 weeks, then a similar use case on a different part of the machine went in circles for months.  This back-and-forth between the SMEs and Data Scientist is fundamentally applied <u>research</u>, not engineering.  Some unpredictability is expected, and I think the best way to deal with it is to set time limits.  After a predefined amount of time (e.g. three weeks), you move to a different use case or provide a very solid justification of why you need to keep working on the current one.

### Development Team and Deployment Team

Once the SME and Data person agree they have a good model, it needs to be deployed.  There are different ways to do this.  At some companies the model development and deployment team are distinct, and the development team hands it off to the deployment team, who puts it into production.  In others, the developers take their development code and make any needed changes so it can be moved to production.

However people choose to do it, there should be a defined process stating guidelines, requirements, and expectations on a software/code and business level (e.g. what needs to be provided to the documentation team, any required approvals, etc.).  Moving the code to production should take a lot less people hours than the development of the model.  
## Data Team and the Alarm/Service Team:

This relationship is actually **the the most important one for the Data team's success.**  alarms are useless unless somebody effectively acts on them.  Even if a model generates perfect alarms, the value is zero (or negative!) unless somebody does something with them.  On the other side, if the predictive quality of the alarms are mediocre, the Service team will lose faith in them and ignore them.  While it's easy to think the SME team is the customer because they propose the use cases and approve the models, the customer is actually the Service team because they use what has been built.  If your customer is unhappy, they won't act on the alarms. If that happens, the Data team has failed.

The working relationship between these two teams is oriented around a defined process of what service needs to successfully act on an alarm.  For example, they might say every model/alarm needs to come with a predefined set of diagnostic and action steps a service technician needs to do.  This could also include educational training so the service personnel understands the purpose of an alarm and what to do with the predefined action steps.  It's then the responsibility of the Data team, SME team, and whomever else, to make sure these deliverables are created before a model goes into production.  If the Data team wants to succeed, it's their responsibility to ensure the process steps, deliverables, and information given to the Service team enable them to successfully act on the alarms. 

The technical and business processes between these two teams should be defined upfront and should be revisited every 3-6 months.  This should include a quality review of the alarms from the perspective of the Service team.  Waiting longer than 3-6 months means any flaws in this process risk becoming festering wounds.  So it's important to bring these issues to light quickly.

When things are going well,  there should be far less interaction between the Data and Service team when compared to the Data and SME team.  The "normal" working interaction should be following the existing process steps as the Data team moves something to production and the Service personnel take over.  Divergences from this should typically only occur if there is an issue with the process and the teams are working together to fix it.
## Governing Team and Everybody Else:

In theory, the Governing team has two broad goals.  
- Defining how all these teams are going to work together. 
- Defining guidelines and processes so the overall project can succeed.  

 To perform those two points successfully, there should be success criteria, such as quality metrics for models/alarms and what quality level is acceptable for a production model.  They should also define things like the minimum requirements for the financial and business viability of use cases.  The Governing team should meet once a quarter to discuss all these topics, and have an open forum so people can voice process concerns.  This feedback should be used to fix issues to ensure projects are successful.

A Governing team should also be able to recognize, or at least acknowledge, the size and importance of a problem and the time and organizational investment required to address it.  It's easy to see small problems as big ones due to a loud voice, or big problems as small ones because nobody understands the scope of it.  An example of this is the Governing body asking a cross-functional team to define a quality metric to decide when a model is acceptable for production.  This seems like a small task, but with different incentives, politics, and different perceptions about data quality across organizational boundaries, this seemingly small task can turn into prolonged war.  It's important for the Governing body to be able to step in and recognize this is an important problem they need to help solve, and not just treat it as a small ignorable side-effect of corporate politics.

In my view, the Governing team should meet no more than once a month to review the current working process and to allow people to formally raise concerns.  If there is an issue requiring further work, a working group (who meets more often in the short-term) can be formed to address it.  If the Governance team is meeting very often (e.g. every week) for an extended period, your process is broken.  The whole point is for them to come up with a process for the different teams to effectively work together without constant intervention.  If the Governance team members need to meet with the other teams every week to deal with process issues, it means your process isn't working and the process for coming up with your governing framework isn't working either.  It's okay to have a two week recurring meeting when you first start a predictive maintenance program and you need to make sure the  framework is right.  But if you are still meeting every two weeks a year later, maybe your governance team needs their own governance team.  
## Hidden teams and Everybody Else:

In my experience, the interaction between the Hidden and Data teams is very task oriented.  For example, if there is an authentication issue on a cloud cluster, a ticket is filed and somebody from the Cloud team supports this.  Once that issue is resolved, the interaction on the task stops.  The duration of these interactions depends on the size of the issue.  For example, an issue requiring a longer fix is when the documentation team needs to update the action plan for every single model deployed in the last three years.  This interaction will last longer than smaller issues, but it still stops once the task is complete.  In the grand scheme of things, the interaction with these Hidden teams is a small fraction of the total interaction with the other teams.

# Pricing 

Let's now go back to how I started this post.  To accurately estimate how much a predictive maintenance program costs, you should take the following things into account.
- The full or fractional salaries of 
	- SMEs (maybe teams of SMEs)
	- Data People
	- Service Team
	- Other teams needed to support the effort
- To calculate the fractional salaries, you'll need to account for time spent in
	- Meetings internal to each team
	- Meetings between teams on short-term topics (e.g. developing a use case)
	- Alignment meetings and Governing body meetings for longer-term topics
	- Time spent following processes, completing templates, and other annoying things of this nature
- Support Costs, which include
	- Fixing issues with old models
	- Updating existing models due to data changes
	- Updating infrastructure code to deal with upstream and downstream changes
	- Responding to support tickets (e.g. a broken dashboard or alarm that seems weird)
- Infrastructure Costs, which is the compute/storage and/or hardware/software costs of running all of this

Just to be clear, when I say "estimate the cost", I don't literally mean you sit down with a spreadsheet and insert items like "Person X =  hourly salary * 2 hours of meetings + ..."  This is simply an order of magnitude analysis to see if all of this makes sense.  If you have a low complexity project setup, your team is two people, and your infrastructure costs are $10/month, then having a use case that saves $500k a year makes sense.  For a high complexity project, saving $1 million a year doesn't make sense.  When you have a program with lots of teams and people, even saving $5 million a year might not be worth it.

# Bad Success Criteria will Kill Your Program

For any predictive maintenance projects to succeed, there needs to be a well defined success criteria so everybody understands the goal.  If your success criteria is not clear or is something vague and long term (e.g. the total target cost savings over two years is X million dollars), individual teams are going to interpret this in a way favoring their own metrics, and a lot time is going to be wasted in constant realignment.

A concrete success criteria will be based on the actual service and alarm data.  This will enable you to properly estimate your costs and cost savings for each alarm.  It should have points like this.
**Before Developing  a Model**
- To be approved for development, each use case needs to clearly show at least $75k in savings
	- This needs to be shown from a time and materials perspective
- What is the dollar value of a good alarm (true positive).  In other words, when the model correctly predicts an future issue, what is the cost savings of proactively addressing the issue?
- What is the dollar value of a bad alarm (false positive).  In other words, when the model predicted an issue would occur but it did not, what would it cost us (e.g. the Service team) to perform an unnecessary service?  
- What estimated threshold of good to bad alarms is acceptable?  For example, if the only way for a use case to financially work is to have 97% good alarms and only 3% bad alarms, I can almost guarantee you will not hit it.  Service data tends to be messy, and it's extremely difficult to reach that without extremely clean or manipulated data. 
- **Do all the teams agree these estimates are accurate?**
**After Developing a Model (Proposing it for Production)**   
- Back testing over six months of historical data, add up the cost savings from the good alarms and the costs from the bad alarms.  Are you seeing the expected savings?
- **Do all the teams agree these estimates are accurate?**

Notice I've included **"Do all the teams agree these estimates are accurate?"** both before and after the model is developed.  This is because different teams can have wildly different interpretations of value, even when they are looking at the same data.  I've seen cases where a Service team has looked at a use case with a seemingly good cost savings and said "Due to the time required to fix this, we don't have the personnel to proactively fix this issue.  We will only fix this during quarterly maintenance when we take the machine apart anyway."  Insights like this can completely change the value of a use case.  It's important for the teams to actually agree on the value before anything is built.

A bad governance structure can be a big hinderance in this step.  In my experience, Governance team members tend to be managers or people with managerial like roles.  This means their jobs force them to focus on KPIs and direction, and less about details of those KPIs.  They then delegate the details (e.g. how do we estimate the value of a use case) to sub-teams, and inter-team politics takes over.  It's really important for the Governance team to ensure all teams come up with a measurable success criteria everybody agrees to, and they step in when that criteria needs to be updated.

I also think it's really important for an honest data person to be part of defining the success criteria.  A data person is the only one who can give you an accurate picture of what can truly be achieved based on what they are really seeing in your data.  If you don't have this type of data person, two or three pilot projects might have to be done so somebody can get the experience needed to provide an informed opinion on the success criteria.   

Doing pilot projects to establish a baseline success criteria might not be appealing from a time and costs perspective, but it will lead to more successful projects in the future, better morale (nobody is motivated to work on projects with bad success criteria), and less wasted time in "alignment."
# How Does all of this Apply to a Low Complexity Project?

Since a low complexity project has far fewer people overall and maybe only a single small cross-functional team, a lot of what is stated above isn't relevant.  What is important is the success criteria.  Even if a project only has two people, it's important for them to agree on a target. This is the only way for both of them to know what they are striving for.  If, after meeting four times, the success criteria is "Well we could build a model to detect this, or maybe this other thing, and there's a third thing maybe" you actually have no success criteria.

Defining a success criteria doesn't mean you cannot pivot.  Sometimes you spend two months trying to predict a particular issue and realize you cannot predict that issue, but you learned you can build a model for something else.  That's okay and you should pivot.  Having a defined success criteria also doesn't mean you can't refine it.  If you start with "Our predictions need to be 90% accurate" but you realize only 80% is achievable and still a financial win, it's okay to shift things.

It's also important to recognize while alignment across teams is not an issue when you have one team, agreement with management is still important.  SMEs and Data Scientists don't randomly do projects together just because they can.  Typically a manger or somebody else approved the project and reviewed the financial and time costs and goals.  So it's important to at least chat with this manager about your progress, success criteria, and get feedback, especially if you pivot. The last thing you want is your success to become a failure because they expected you to do something different in comparison to the work you actually did.
# Miserable Data Teams
 
This is a tangential topic, but this post is the right place for it.  If you talk to people in Data teams (e.g. Data Scientists and Data Analysts), you might meet frustrated people.  Online forums are filled with data people who express a lot of frustration at their data jobs, usually tied to misaligned expectations and not getting the support they feel they need.  I'm not going to try to establish a single cause for this, but I would like to point out something.

Data teams are ones who see and feel all the dysfunction arising from different teams of people working together when they all have different goals/metrics, and are sometimes incentivized in opposing ways.  This is contrast to people who only see the frustrations in their own org, and are somewhat isolated from the chaos around them.

An example here might add clarity to this.  At one point I had a job in a customer facing role at a company with a lot of different products.  For reasons not worth talking about, one of the SaaS products my team supported was struggling in the market. Our entire team was miserable because we spent our days with product issues, frustrated customers, and basically ping-ponging between different internal groups (e.g. sales, product management, engineering management) trying to convince them to change direction because we could see with all the opposing forces and goals of each group, the customer was forgotten.

Our state of misery was in complete contrast to developers in the Engineering org.  We'd complain about an API making no sense to use it the way they expected customers to use it. They'd brag about how the API could handle X million requests per second, how they provided seamless authentication, and how they solved all these interesting technical problems to do this.  They had no idea nearly zero customers had been willing to pay for the service after trying it.  The developers were buried down at the bottom of a large Engineering org, and to them they were hitting their goals and they were happy.

Many Data people see problems across organizations.  You can be involved in a project spanning three orgs, and realize poor alignment across these orgs means this project is going to fail.  But you still have to work on it.  And if you do something to make one org happy, the other two are miserable and you are the face of that.  The data person becomes the focus point of all this dysfunction because they are only person who understands how all the pieces are supposed to fit together.

The flipside of this is Data people can have a unique view of the organization and what is needed to move things in the right direction.  Some Data people are exposed to everything from analyzing individual rows of data to looking at organizational level metrics, targets, and motivations.  They have what is equivalent to a sociological understanding of what is really going on.  They can map how the micro is supposed to feed into the macro, and why things are not working.  With the right support, they can help inform leadership about out what's broken and needs to be fixed.
# Wrap Up

The process different teams use to work together will define the success of your predictive maintenance efforts.  And it's only when you have a good success criteria that you can define a good process. The best path to the wrong destination is still the wrong path, and if you can't recognize when you have bad success criteria, you'll never be able to understand why your process keeps leading to failures.

I've worked on, and been involved in, a lot of predictive maintenance projects, and very few have failed because of a technical decision or the choice to use a simpler algorithm versus a more complex one.  If a lot of your projects are not showing any value after they make it to production, or the majority of your projects never make it to production, I'd really recommend you step back and investigate your process and where things are going wrong.  Somebody probably knows what the issues are, but what they are trying to say is getting lost in corporate noise.











