+++
title = 'Is Your Predictive Maintenance Data Science Program Worth it?'
date = 2024-02-26T00:00:00-04:00
draft = false
mermaid = true
+++

One mistaken assumption I see people make with predictive maintenance programs is that the cost savings of many lower value projects will add up to a big number, so it's worth putting in the effort.  But sometimes saving a million dollars isn't worth it if it costs you more than that to implement it.  In this post we'll look at four types of programs and understand why some of them aren't worth doing because they have a negative return-on-investment (ROI)

<!--more-->

First, an anecdote.  At one of my previous jobs at a very large company, the CEO announced a contest for people to come up with business proposals on how the company could save money.  The CEO tasked SVPs and their teams to come up with ideas and estimate the cost savings of each.

Our SVP got us together to discuss, and something he said completely shocked me.  "If you're going to propose something requiring a business org outside ours, it needs to save at least a million dollars.  If it's less than that I don't want to hear about it because we'll spend more than that to implement it."

This completely blew my mind at the time.  Was he really saying this company was so inefficient that it would cost the company at least a million dollars of people-herding to try and save money Was this how large businesses really worked?

This post is basically an extension of this anecdote, because anybody with work experience in a large company knows what the SVP said is true.  In predictive maintenance, I've seen many instances where people assume once things scale, their predictive maintenance programs will be worth it.  But friction between different parts of the company means the program never scales, and all you have is a lot of impressive PowerPoint slides with nice numbers, but the numbers never add up in the real world.

# Predictive Maintenance and Failure Modes

To make sure we're all thinking about the same type of project, let's define the use case(s) this post is referring to.  As stated in the title, this is about predictive maintenance.  And by predictive maintenance, we are referring to the idea that we want to predict when a machine fails in defined way and mitigate the effects of this failure, also known as a **failure mode**.

A **failure mode** is another way of saying a specific type of failure.  For example, if a plastic hose cracks and leaks, the hose cracking is specific type of failure mode.  Note we can only predict certain types of failure modes.  If somebody accidently drops their phone into lava and the phone melts, the melting due to high heat is also a failure mode.  But that's a very atypical failure mode, and not something we are going to expect.  A simple machine (e.g. an automated can opener) might only have a limited number of failure modes.  A more complex machine might have thousands of parts and thousands of failure modes.  

The cost of **failure mode** is highly dependent on context.  A leaking pipe might leak a bit of water onto the floor and not cause any short term machine issues at all.  Or if it leaks onto a circuit board and causes an electrical failure, the entire machine can stop working and that is a very high impact failure mode.  This context gives us the information we need to define the value and severity of each failure mode.

Typically, but not always, predictive maintenance models are focused on predicting or detecting known failure modes.  The reason for this is if a model predicts a known failure mode, a subject matter expert knows what to look at to validate the issue and what to do to fix it.  This greatly accelerates the mitigation or resolution of the impact of that failure mode occurring. 

At least in my experience, the more general purpose use cases providing a generic "something is wrong with the machine" aren't very useful from a business perspective.  They are impressive in slides (AI based anomaly detection!!), but it is difficult to take any sort of concrete action based on that information.  Imagine if your car had a "something is wrong with your car" notification, but no additional information beyond that.  Would that be useful to you?  Would you pay extra for that "Smart AI" feature?  Probably not!

# The Predictive Maintenance 4-Box

Now that we've defined **failure mode** and its relevance to predictive maintenance, let's classify these failure modes on two axes.  First, a specific failure mode can be **high** or **low** business value.  Business value is defined as **R**eturn **O**n **I**nvestment, which means however you measure it (cost savings, increased sales, profits, etc), that is how much more value you generate than it cost to do it.  Just for concreteness, let's say a **Low Business Value** failure mode is **$50K US Dollars or less in ROI.**  A **High Business Value** failure mode is **$150K US Dollars or more in ROI.** These numbers may be smaller or bigger depending on the company and use cases so don't take them as set in stone. 

A business can choose to focus on anything in the range of a handful of high value failure modes to a lot of lower value failure modes, or even a mix of both.  Where they focus their efforts will determine how much of a ROI they can realize.

To create a models for these failure modes,  the effort required exists on a spectrum of needing a lot of **manual** work with a lot of customization, or they can be fully **automated** with little human intervention is required.  Most projects will probably be in the middle, requiring a mix of both.  I'm going to talk more about how this impacts ROI soon.

Let's plot all of this in a 4-box.  This chart might seem extremely obvious to many readers, but you'd be surprised how many teams and projects end up in the bottom half of this chart while assuming they are in the top half.

<div class="mermaid">
%%{init: {"themeVariables": {"quadrant3Fill": "#fe8487"} }}%%
quadrantChart
    title Is your Predictive Maintenance Program Worth Doing?
    x-axis Manual Model Development --> Automated Model Development
    y-axis Low Business Value --> High Business Value 
    quadrant-1 $$$ Profit $$$
    quadrant-2 Might be worth it
    quadrant-3 Don't Do it for the money
    quadrant-4 Might be worth it
</div>


It's important to recognize this chart is summarizing an entire predictive maintenance program, and not just a single failure mode.  If you have twenty use cases and each of them has an ROI of $75K, the total ROI of your entire program is $1.5 million.  On the other hand, if you have one use case with an ROI of $250K and five others, where each turns out to have an ROI of negative $50K, maybe it's not worth it even to do the $250K one because it'll cost you more than that to implement.

I'm going to talk about each quadrant in more detail, but let's take a tangent first.  I think the challenges with automating things can be greatly underestimated, and it's one of the major ways I've seen programs slowly slide into a negative ROI.  So it's worth it to clarify what automating model development entails, and where the challenges are.  

## How much Effort is needed to Develop a Model?

**Manual** and **Automated** Model Development is really about the people hours needed to build a model.  A model might be anything from a simple set of rules to something complex with deep learning.  However, model building is not just about the actual model.  To build a good model, you need the right data and right context.  And once the model is built, you need to integrate with other systems (e.g. a repair ticketing system) so customer facing people (like machine technicians) can use the model predictions/outputs to make decisions and do something.

To understand what is meant by manual vs automated model development, it's useful to have a high level overview of the steps to build a useful model and deploy it into a production setting.  In the list below I've bolded the steps that can resist all forms of automation and be so time consuming there's simply no way to have a positive ROI. 

1. **Correctly identifying the problem that needs to be solved and defining a solution with a positive ROI**
2. Data work
	1. **Data curation (finding the right data that identifies the problem)**
	2. **Data cleaning and preparation**
	3. Building a data pipeline
3. Building a model
4. Deploying the model and generating output/predictions
5. Integrating with external systems 
6. **Training and working with the people who perform actions based on the model output**

The first point might surprise people, but I've frequently seen situations where people can describe a high level problem with high ROI potential, but once it's broken down into a tractable problem it loses most of the ROI.

For example, let's say the problem is "low fluid pressure." When trying to turn this problem into something that can be solved using data and a model, it turns out there are multiple causes and fixes ranging from changing a leaky pipe (easy quick fix) to replacing a major component of the fluid system (a harder fix that takes much more time).  What was a single problem with a previously idealized single solution has now turned into multiple solutions.  Multiple solutions may require multiple models and those require all the people-hours required to build them.  There is also the possibility some of those models don't work out because of poor performance, resulting in a further decrease of the overall ROI, or even pushing the use case into a negative ROI.

The second point (about Data) in the list is really about ensuring a clear mapping between the datapoints being collected and the problem.  First, you need to be able to clearly identify which datapoints (out of all data being collected) provide the signal needed to detect the problem.  Second, you need to be able to clearly identify when the issue is occurring (abnormal operation) and when it's not occurring (normal operation).  This data mapping can be very difficult to scale, meaning for every problem you are trying to capture and predict, the data mapping has to be done manually and with a lot of back-and-forth with different SMEs.  

The last point is all about the human processes downstream of the output of a model.  A prediction from an model indicating an issue might occur isn't useful in itself.  There has to be a process for that prediction to be turned into a concrete set of actions people do to diagnose and fix (if needed) the machine.  That means all the downstream systems need to be able to process the model output, an action plan based on that output needs to be well defined, and the personnel acting on that plan need to be correctly trained to perform it.  Any flaw in this process can result in incorrect or unnecessary actions or misdiagnosis, driving up costs instead of reducing them.  If every action plan for every model has to go through an iterative process to create and get it right, that creates more manual work which drives up costs.

## Automation And Scaling

The primary value of automation comes from being able to increase your ROI by scaling what you have built.  There are two ways of scaling relevant to this post.  The first is having a model with enough flexibility to deploy across a fleet of machines.  This type of scaling works well because you can achieve a high ROI when you add up the total cost savings from each machine.  One of the benefits of this type of scaling is since you are focused on one model/failure mode, it's easier to fail fast and recognize when a model can't be built.  For whatever reason, there seems to be less ego in acknowledging failure here since you are only letting go of a single use case.

The second way to scale is building models for multiple failure modes.  With this, the value comes from being able to add up the cost savings from the solution to each failure mode.  Even if the ROI for each individual solution is lower, the total ROI is higher if the models can be built and deployed without a lot of manual work. In my experience this is where, unfortunately, a lot of people misjudge how much they can automate the development of each solution for each failure mode.  And since the plan was for multiple failure modes to combine into acceptable ROI, it takes much longer for people to recognize and admit the numbers just don't add up.  I'll talk more about this in the next section.

# Exploring the Quadrants

Now that we've defined the axes terms and we have an understanding of scaling and how manual work can drive up costs, let's discuss the different quadrants.  

## **High Business Value** and **Automatable**

In this quadrant a typical use case is a high value fault mode occurring often enough that building an automated solution lets you scale the cost savings across machines.  The "automated" part of this doesn't have to be the development of the initial model, but it's more about building variations of the model (e.g. for machines running in different environmental conditions), model retraining, etc.

An example is seen with oil and gas equipment deep underground.  Simply accessing the machine underground, let alone repairing or replacing it, can cost a lot.  Let's say there is an issue resulting in a broken machine, which costs $150k to repair/replace, which includes lost production costs due to downtime.  You have 500 machines worldwide, and in the past you've seen an average of 1 of these fault modes a year per machine.  An intervention can be done to mitigate this, but the complex set of conditions causing the fault need to be accurately detected for the mitigation to work.  Since the machines are running in different parts of the world, the environmental (e.g. the ground is mostly sand versus a lot of mud) conditions are different and a different model is required for each set of operating conditions.   

It doesn't take a lot of math to see even if an early detection model only accurately catches 50% of future machine failures, that still leaves a large margin to invest in building and operating a solution with a high ROI.  And if more machines are added, the cost savings automatically scales.

One question people could ask about the quadrant is "I have ten failure modes that _theoretically_ add up to a high ROI.  Would that fit into this quadrant?"  This question can only be answered after you start implementing the solution(s).  If you implement the first two manually, and then use that knowledge to automate the implementation of the next 8, and if your ROI numbers turn out to be accurate, then yes, you are in this quadrant.  The risk with 10 different failure modes is that there are many opportunities where your ROI expectations are incorrect or the automation simply doesn't work.  With ten failure modes it will take you a lot longer to determine if you can build the solution(s), as compared to a single high ROI fault mode where you are likely to figure that out much faster.

## **High Business Value** and **Manual Model Development (Resists Automation)**

There is a type of problem that is very high value, but developing an automated solution is difficult, resulting in no opportunity to scale.  This can happen because you have a very small fleet of specialized machines and there's limited opportunity to multiply the cost savings of each machine.  This can also happen if the data is very dirty and requires a lot of human provided context to clean, providing limited ways to automate and share this cleaning process across use cases.

This can also occur from a low sample size of your problem.  If there is an complex intermix of events causing your whole factory to shutdown for a week, but it's happened three times in the last year, it's hard to have enough data to separate the abnormal from normal operations.  You recognize solving this problem can save a lot of money, but the cost of solving it is completely unknown.

You can also be in this quadrant when the ROI shouldn't be measured purely from a financial perspective.  It could be a product safety issue resulting in injuries or death.  Or it could be something that destroys the reputation of the company.  Some programs in this quadrant are done to appease regulators or because the company got in trouble in the past.

Even if a highly automated solution cannot be built, a possible alternative is a mix of human decision making being informed by models.  A single or set of model(s) can provide insight into where to look, and pre-built data analysis tools (e.g. a highly interactive dashboard) provide the mechanism to how to look.  Data Scientists/Analysts and Subject Matter Experts use these to unravel what is going on.  That mix of brain and machine are what it takes to catch the problem before it happens.

This just speculation on my part, but when I've seen business pursue this type of problem, the cost savings, or potential cost savings, is well over $1 Million.  In other words, the problem is worth enough that you're willing to tolerate a complex mix of pre-built tooling (the automation) and also allocating a team of people to investigate manually.

## **Low Business Value** and **Automatable**

In this quadrant, and also the bottom half of the square, you've identified a number of lower value fault modes, when added together have a _positive_ ROI.  In other words, a handful of these use cases on their own aren't worth it, but a lot of them together make sense.  This quadrant could also be called the big box retailer strategy, which is to make a small profit on each item sold and rely on high sales volume to add those small bits together to make a big profit.

The key to succeeding here is automating the model building process so the solution for each failure mode can be built quickly.  To do this, there are two key things that have to be in place
1) Well curated data with good labels, which clearly indicates when the failure mode has occurred and when it has not occurred. 
2) A fast and repeatable process for translating SME knowledge into a solution (e.g. Data Engineering and Model code).  This is essential for mapping the failure mode to the right data that identifies and precedes the failure.

If your data is well labeled and mapped, you can build ways to automate the data engineering for the training data.  This means you can quickly identify the right data for all the failure modes you are building models for, and not get bogged down in a lot of manual data cleaning work along with excessive back and forth with SMEs.  If your data is really bad, having the best SMEs in the world isn't going to save you.  A clear sign that you are in the next quadrant (you cannot automate development), is if you have spent six months working on solution(s) and you are still bogged down in a lot of manual data work to build the training data for each use case.

Regarding the second point in the list above, it's also common to be in a situation where once you start working on a use case,  it becomes clear there is a lot of additional context needed nobody initially realized was important.  It can take weeks or months of discussions to uncover all this context because it takes that long to discover all the right questions to ask.  It's expected for the first few use cases you implement, the process is going to take more time and have more friction.  Initially you have to learn how to streamline the process of defining use cases, the communication strategy with SMEs/stakeholders, and identifying the right data.  But if after the tenth use case this process is almost as manual and cumbersome as the first four, you should make sure the process can really be streamlined or if you'll never be able to automate it.
 
Something I've seen happen is the technology parts of these processes get automated, but the human parts never get streamlined beyond people building more trust with each other and creating some PowerPoint/Excel templates for defining the use case.  The end result is the infrastructure to develop and run these models in production and the integration with the service ticketing system is ready to go.  But the parts where the use case and "win" is clearly defined, the data is clearly identified and well labeled, and service personal are trained well enough to handle the alarms, is never streamlined.  What you end up with is a system ready to scale, but it never scales because you cannot automate the hard parts.  From there you fall directly into the next quadrant.
## **Low Business Value** and **Manual Model Development (Resists Automation)**

This is the quadrant I'm going to write the most about because I've seen many companies end up here and not really recognize (or admit) it, even if some individuals involved in the project realize they are here.  This quadrant is also not limited to predictive maintenance or industrial machines.  People in other industries will recognize it as well.

In this quadrant, you are unable to scale the development of your lower ROI use cases because of human and data issues.  You never achieve your projected total ROI because you spend too much time and money developing solutions for each failure mode.  In the end, your ROI is actually negative because you spend more money building models and dealing with issues than anything you actually save.

Let's look at some numbers.  Assume each of your use cases generates a *projected* $50K cost savings.  You've been working on these for a year with a team of two data scientists and one data infrastructure person. It takes about two months for each use case to move into production.

After the first year, you get ten use cases into production, meaning you recognize $500K in cost savings on some spreadsheet.  Keep in mind it will take some time to see the actual cost savings materialize in the field, so this $500K is really just a forward looking estimate.  This cost savings isn't even going to pay the salaries/benefits of the three team members, let alone all the other infrastructure and support people needed to realize that cost savings.  

When you started the project you expected for the first two years the ROI will be negative, but after that the math will work out.  In year two you complete ten more use cases.  In year three, ten more.  Now you are saving $1.5M, which looks a lot better. Plus, over three years, the number of machines out in the field is projected to increase, so this savings will be even higher.  

However, there are different reasons these cost savings numbers can be misleading.  Let's look at a few.

Let's talk about true and false positives.  A true positive is when your model **correctly** predicts a failure mode is going to occur.  You realize a cost savings when somebody acts on that prediction and reduces or removes the impact of that failure mode.  A false positive is when this prediction is **incorrect** and that failure mode is not actually going to happen. Every false positive means a person spends time and money diagnosing or trying to fix a problem that doesn't exist.  The number of false positives scales with the number of machines in the field.  More machines means more false positives, which means more costs.  Every false positive costs money, and if the ROI wasn't very high to begin with, the ROI becomes negative very quickly.

Another issue with false positives is at a certain point, dealing with them makes people distrust the entire system.  If people don't trust the Alarms, they won't even trust or act on the true positives.  If this happens, all the potential savings goes to zero. 

How bad the "rate" of false positives feels is also affected by perception.  If you have one false alarm out of ten alarms, it doesn't feel bad.  If you have ten false alarms out of hundred, or hundred out of a thousand, the percentage of false alarms is the same in all cases.  But if you are the person acting on the alarms and dealing with the pain of checking ten or a hundred bad Alarms, your perception of the model is bad.  So it's not just about the percentage of bad alarms, but really how much impact these are having on your field personnel.

Another factor that can decrease your ROI is predictive accuracy degrading over time with any hardware (e.g. a frequently failing part is reengineered) or software changes with machines.  The more models you have, the more effort is required to monitor them and make sure they are performing well.  Perhaps your Data Science team targets to release ten new models a year and you do this the first year.  But in the second year, your development slows down because you need to ensure those ten models continue to run well.  The effort to review and maintain older models only increases over time as newly released models inevitably become old models.  All of this will either slow down the rate of which you can address new use cases or you have to grow the team, which will increase the costs.

There is also an opportunity cost to a predictive service program.  Beyond the core development team, there's the time investment of all the other people (e.g. SMEs, Data Engineers, Service Engineers) to support this.  They could have spent their time doing something else, so it's important to account for their costs when justifying your program.

All of these issues (and many others) mean your original ROI estimates are very optimistic. They might assume a perfect system that identifies every issue without errors, and every issue is dealt with perfectly by the service personnel.  That $1.5M estimate for the third year can get cut quite drastically and you might not even get a third of it.  If you are a billion dollar business, is it really worth investing in a project providing $500K annual cost savings after three years?  You could fire your Data Science team and save that money with much less effort.

How do people end up in this quadrant?  What I've seen happen is a predictive service program being conceptualized with some number large (e.g. $10M/year) enough for upper management to take it seriously. Then, as the program is implemented, the actual savings is not well tracked and people refer to the original financial projections when asked.  As employees and managers change, teams reorg, and priorities and ownership change, the line from that original number to today becomes even more blurry.  After two years, nobody can clearly state where that original number came from and how it ties to the current state of things.  But it's still a target, so people stick their head in the sand and try to hit it. 

Companies can also land in this quadrant when they need a predictive service program for strategic reasons.  If your competition claims to do "predictive service" it's going to be challenging for you to tell customers you don't have it or you don't believe in the value of it. Sensible strategy turns into dysfunction when the only way to justify this type of program is by making claims of a large financial advantage.  I've seen this at larger companies, where the only way to get multiple departments (e.g. machine engineering, data science, customer service) to work together is to get buy-in from upper management and the top of the middle-management layer.  A bold financial benefit claim is made and set as a target, resulting in a program which is doomed from the start.

If you are in this quadrant, getting out is tough.  Simply dissolving or watering down the program and rebooting it a few years later with newer people and technology seems to happen often.  As much as I dislike the nastiness that comes with doing this, if something is a big enough mess, then destroying and rebuilding it might be easier than trying to fix it.

If you do want to climb out of this quadrant, I'd recommend properly identifying the root cause(s) of the struggling program instead of helicoptering in new people to fix it and giving them deadlines not grounded in reality.  They're only going to start covering up the symptoms instead of understanding the root cause.  

What are the root causes of this type of program going off the rails?  They can be quite varied, including
1. Different stakeholders having different interpretations of how bad the data quality issues are and what the real impact is on reducing the ROI.
2. Stakeholders having inconsistent definitions of business value, meaning one stakeholder thinks a problem is worth $100K, whereas another stakeholder feels this problem has negative ROI.
3. Too much of a top-down approach, where the most "valuable" use cases that <u>have</u> to be implemented (nobody can say no) are defined in a very academic way, without a robust data analysis of actual field data to support those ROI values.
	1. Overly focusing on checking as many "we got it into production" boxes as possible, resulting in high ROI in a spreadsheet, but not in reality.
4. Minimal overlap between use cases or fault modes, making it difficult to share knowledge or share development methodology/code, which reduces opportunities for automation and scaling.  Every solution then becomes a unique snowflake.

Notice all the points above are really about alignment between the different people involved in the program.  Even the fourth point is really about making sure people understand use cases and the associated context need overlap to be scalable.  A Data Scientist might understand, but people who are not data professionals might not realize the extent to which overlaps are needed to support automation in a data and software process.  If different stakeholders aren't aligned, even declared successes by one group can be perceived as failures by a different group.

I'll finish up this post by talking about what gave me the idea to write it.  I've seen too many instances where a program failed and people pointed their finger at the symptoms instead of the cause.  It's easy to scapegoat the data science team, but they're usually only the face of deeper organizational problems.  I hope this post provides insight into some of the broader factors that drive success and failure in predictive maintenance programs, and you use them to better triage and diagnose any issues you are having.  If nothing else, you know more about what types of projects to avoid.



































