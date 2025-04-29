+++
title = 'False Alarms can Erase the Value of your Predictive Maintenance Efforts'
date = 2024-05-20T00:00:00-04:00
draft = false
mermaid = false
+++

People often underestimate how false alarms from predictive models can erase all the business value you set out to create from those models. In this post we will explore how label noise can drive up your service costs instead of decreasing it, and how a small fraction of incorrect predictions of impending machine issues can erase all the benefit created by correct predictions.

<!--more-->

In my experience, many predictive maintenance projects follow this pattern.

1. A financial analysis is performed to understand the current cost of servicing a machine or a fleet of machines.
2. People look at different ways to reduce these costs, and will estimate how much **proactive service** (e.g. fixing issues before they happen, better scheduling of predicted service issues, etc) will reduce costs.
3. If the estimated cost savings of proactive service are high enough, a predictive maintenance program will be proposed.
4. An internal and/or external team of Data people will be created to build the infrastructure and models (or rules) to predict future machine issues.
5. Models will be built and deployed for a year or two before the issues and cracks in value realization start to appear.  This is assuming some external event (e.g. a recession) doesn't force an early analysis of what is going on.
	1. The competent data people will start to recognize the theoretical financial benefit of each model is not being achieved.  The honest ones will start openly grumbling about it, maybe not in the most productive ways.
	2. The lack of cost savings becomes apparent to service management.  How quickly they realize (or acknowledge) it will be highly dependent on many factors.  For example, if there are a lot of false Alarms causing a significant increase in costs, this will be recognized faster.  Sometimes the growing effect of false alarms is more insidious and it takes longer for people to realize what is going on.
6. At this point people will conduct a more rigorous analysis and realize the predictive models are not reducing costs.  One benign scenario are the models generating zero or a very low number of alarms, so there is little or no proactive service occurring.  A more damaging situation is if there are a lot of incorrect alarms and people are doing work that doesn't need to be done, actually driving up service costs instead of reducing it.  We'll talk more about this later in this article.
7. To help fix the situation, one way forward is for the service and data people to work together to come up with a more accurate way of evaluating the real financial benefit of each model.  They then rework or scrap existing models.  In a worst case scenario the entire predictive maintenance program is scrapped, people are laid off, and nobody really understands the root cause, dooming this whole process to be repeated in the future. 

This article is about the latter case in point 6. My goal is to provide insight into how bad Alarms, or false positives, from predictive models can drive up service costs, even in the case where a model is providing a lot of good/accurate Alarms.  This will hopefully, help shorten the time between points 4 and 7 above.  In an ideal situation, these types of issues will be identified in step 4 before a model is even put into production, so the service people never see a growing divergence between the theoretical and actual cost savings provided by a model.

The term **Alarm** is used in this article and defining it will help clear up any questions about what it means.  An **Alarm** means a prediction from a predictive model indicating some event or failure is going to occur.  Predictive Models can also predict something is not going to occur, but this is not be considered an **Alarm** in this article.  **<u>Every prediction is not an alarm. Only the predictions of some undesirable event happening are considered an <b>Alarm</b>.</u>**  

# What is Label Noise?

Imagine you have a machine with a water hose, and one of the ways this hose fails is by cracking and leaking.  As a Data Scientist, you are asked to predict this issue before it occurs. To do this, you go into the machine service data to find historical examples where this issue has occurred so you can see what the machine data looked like right before the hose cracked.  You can then compare this to machine data where this crack did not occur.

The service data is the historical record of issues customers have had with their machines, and what work was done to fix these issues.  For this particular issue, when a service technician goes onsite to fix it, they enter the cause of the issue in the service ticket, and what they should be entering is some variation of  "cracked hose."  However, during at least one instance where this issue occurred, the technician entered "fluid leak" instead, making it challenging to clearly identify if it was the hose or something else that was leaking.

When you train a model on this data, what you are doing is providing the model with examples of when the pipe cracked and when it did not, and tasking the computer to learn the difference between the machine conditions when cracks occur and when they do not.  The more examples you have, the better the model is able to determine the difference. 

The examples and data you provide to train the model are typically created (e.g. Feature Engineering) by an Analyst or Data Scientist, and they are based on the information in the historical service data.  However, when the service data has errors or lacks clarity, you can end up in a situation where the examples are incorrect.  This results in the model learning the wrong things from the examples, which leads to incorrect predictions.  With more incorrect examples, you'll get more incorrect predictions.

Each example used to train this type of predictive maintenance model has a **Label** indicating if an issue occurred or not.  And this **Label** is how the model's algorithm can tell the difference between machine data indicating a crack will occur, or if it probably won't occur.  In an ideal world, the labels would be 100% correct.  But as you can see from the example of the service technician incorrectly entering the cause of the issue, it's extremely difficult to ensure all the labels are correct.  This results in **Label Noise** in our data.

You might ask, "Can't a Data person manually check the labels to make sure they are accurate?"  It might be possible to do this with a very simple machine, but complex machines can have a lot of machine and sensor data, making it impossible for a person to manually check.  There could also be an entire fleet of machines, all working in very different environments (e.g. different geographies and climates) making the data more complicated. So there's no realistic way for a person to manually validate every label. 
## Label Noise Sources 

For people who are curious, here are some possible sources of label noise in service data.  

Some possible sources of label noise or incorrect entries in service data.
- Misdiagnosed problems (e.g. thinking a hose is leaking due to a crack, when it's actually incorrectly connected to the machine).
- Problems being diagnosed by trial and error resulting in confusing and contradictory information in the service ticket.
- Using ambiguous words to describe the work that was done.  An example is using the word "fix" when a part is replaced.  Yes, the issue was fixed, but the fix was to "replace" something, not perform some repair on that part. 
- Limitations in the software used to enter the service information
	- Example: A technician has to fix multiple issues, but their ticketing system only allows them to enter one issue per ticket, and they don't want to open multiple tickets.
	- A technician using a pre-populated drop down and being forced to pick the best option, not the right one. 
		- Accidently picking the wrong item in a drop down because they are using a tablet and their fingers.
- Issues arising from multiple languages being present in the data, and words having different meanings in different languages.

The above is not a comprehensive list. Going into the details of where this label noise comes from is beyond the scope of this article, and is sometimes very specific to the type of machine and process you are working.
# The Financial Benefits of Predictive Maintenance

Before I get into the impact of label noise, let's talk about the theoretical financial benefits of predictive maintenance.

It's important to recognize for commercial machines, the baseline maintenance cost is not $0.  Instead, you actually start from a negative.  Imagine you have a machine requiring, at a minimum, a quarterly service which costs $2500 for parts and labor.  That machine costs you $10K a year to run, so it needs to generate at least $10K in yearly value for you to break even.

Let's now say the company building that machine says they have redesigned one of the parts, and now you only need to replace it once every six months instead of replacing it every quarter.  Replacing this part twice a year instead of once a quarter will reduce your costs by $1k.  So now, instead of spending $10K/year, you are spending $9K/year.

In the real world, machines are not only serviced during regular service intervals, but also when things break or are not running correctly.  This is what is known as **reactive service**.  Basically, when there is a machine issue, somebody reacts by fixing it themselves, calling somebody else to fix it, etc.  **Reactive Service** happens as a reaction to some issue with the machine.

This combination of regular and reactive service is your baseline for service costs.  If you find you have $10K in regular planned service and $5k in reactive service, your total cost is $15K.  If that is your cost per machine and you have 10 machines, your annual service costs are $150K.  So when you say you want to reduce your costs, $150k is the baseline you want to improve upon.

Reactive service disrupts normal operations and drives up costs. It's no different than your home cooling system breaking in the middle of summer during a heatwave, as compared to it breaking during a mild spring. In the former case, you will struggle to find somebody to fix it, parts may be expensive or not available, all the while you are living in your uncooled house.  You'll essentially be forced to pay the highest price required to get it fixed quickly.  If the weather is mild and you have to wait a few days for the fix to save a lot of money, it's not an issue. 

This is the benefit of **proactive** and predictive maintenance.  Instead of reacting to issues, you get ahead of issues.  Having this advance information gives you the ability to plan and schedule things in a way that allows you to reduce your costs and the impact on your operations.

Now that we've got the background covered, let's talk about false predictions.
# The Cost of Incorrect Predictions

## False Negatives Might be Okay

Imagine you have a model continuously ingesting all your machine data and predicting if an issue will occur.  This model runs twenty four hours a day, seven days a week, and generates hundreds, if not thousands, of predictions a month.  This model is also terrible, and never predicts any issues are going to occur.  In other words, it generates zero Alarms.  What is the financial impact to your service costs?

The answer is the impact is basically zero.  Having a model that never predicts an issue is going to occur is the same as not having a model at all.  Before you had a model, all issues were dealt with reactively and your baseline service costs were based on reactive service.  If a model always predicts nothing is going to happen, you're only going to fix things reactively after the issue occurs.  So your service costs with this terrible model are the as same before you had it.

When a model falsely says an issue will not occur but it does occur, this is considered a **False Negative.**  We're not going to talk about false negatives any more in this article, but it's good to understand what it is so we can mentally contrast it with a false positive.
## False Positives are the Real Problem

A **False Positive** is when your model predicts an issue is going to occur, but it wasn't actually going to occur.  If you perform service activities based on that prediction, you are basically doing work that doesn't need to be done and replacing parts that do not need to be replaced. **A false positive is much worse than a false negative because unlike the latter, which has minimal impact on your overall service costs, a false positive increases your costs.  A lot of false positives can drive up your costs significantly**.

Let's do some armchair math to understand how much of an impact this can have.  Let's say you have an issue which occurs 100 times a year.  Dealing with this issue reactively costs you $500 per occurrence, or $50K a year.  You look into the benefit of dealing with this proactively, and you conclude that instead of $500, a correct prediction results in your service cost being $250 (parts and labor).  So you save $250 compared to the reactive service cost. However, every false positive will cost you $500 because you are doing unnecessary work, and the parts and labor costs are the same as if you had done this reactively.

Based on those numbers we have the following calculations

| Number of Alarms | True Positives | False Positives | Value of True Positives | False Positive Costs | Total Financial Benefit of the Model |
| ---------------- | -------------- | --------------- | ----------------------- | -------------------- | ------------------------------------ |
| 100              | 100            | 0               | $25000                  | $0                   | $25000                               |
| 100              | 95             | 5               | $23750                  | $2500                | $21250                               |
| 100              | 90             | 10              | $22500                  | $5000                | $17500                               |
| 100              | 80             | 20              | $20000                  | $10000               | $10000                               |
| 100              | 70             | 30              | $17500                  | $15000               | $2500                                |

It's important to notice how quickly the financial benefit of the model gets cut in half.  You might think eighty correct predictions and twenty incorrect predictions is great and the model is doing really well, but it's really not.  The value of the model quickly goes to zero, or even negative, as the number of false positives goes up.

# How Label Noise Increases False Positives

Now that we have discussed the cost of a false positive, let's go back to the idea of how label noise results in false predictions.  I've created a simulation to understand how label noise affects the predictions.  The simulation first creates machine data with labels.  As part of the data generation process, the amount of label noise can be specified.  For example, if 10% label noise is specified, 10% of the "Issue Happened" labels are randomly flipped to "Issue Didn't Happen."  

After generating the data, the simulation trains a model on a training dataset, and makes a lot of predictions on a test data set.  The goal is to generate Alarms and then determine the number of true and false positives.  I then normalize the Alarms to 100 Alarms to make it easier to mentally calculate percentages when you look at the results.  If you'd like to see the code, refer to the [Jupyter Notebook](https://github.com/ujkam/ujkam_code/blob/main/Label%20Noise%20Simulation.ipynb)

Let's start with a model that predicts a single issue.  This model either predicts a zero (no issue is going to occur) or a one (the issue is going to occur).  In the table below, the "unnecessary work" column is the false positives.  I say unnecessary work because the false positive asks us to fix something that doesn't need to be fixed.  

These cost numbers are repeated here so you don't have to scroll up.
Cost of False Positive: $500
Cost of a True Positive: $250
Maximum Cost Savings with 100% True Positives: $25000

| Number of Alarms | Percentage Label Noise | Unnecessary Work | Financial Cost of False Alarms | Final Costs Savings |
| ---------------- | ---------------------- | ---------------- | ------------------------------ | ------------------- |
| 100              | 0                      | 1                | $500                           | $24500              |
| 100              | 5                      | 8                | $4000                          | $21000              |
| 100              | 10                     | 9                | $4500                          | $20500              |
| 100              | 20                     | 13               | $6500                          | $18500              |

You can see the trend here.  As the label noise increases, so does the false positives.  And as the false positives increase, your actual cost savings goes down.  How "bad" this is to your business case depends a lot on what your expectations are.  If your predictive maintenance program only makes financial sense if you fully realize that $25K in cost savings, this table says your program isn't going to work.  Even in the case of 0% label noise, you are going to have false positives.  Nobody has perfect data and you are not going to save $25K (per 100 Alarms) with proactive service.

However, if your program makes sense at $18K in cost savings, your program might be viable.  You just have to ensure your data quality is decent and you can keep your label noise under control.   

Unfortunately, the scenario where a machine only has one possible issue is very optimistic and isn't realistic.  Machines can fail in multiple ways, so let's extend this scenario. Instead of our machine only having one issue we want to predict, we want to predict two issues, issues A and B.  To make the math easier, let's say the costs of issue B are the same as our previous costs for A, which is $500 for a retroactive fix and $250 in savings for a proactive fix.  The model now has three possible predictions; no issue predicted, issue A predicted, or issue B predicted.

**Adding this issue B changes our conceptualization of a false positive in a seemingly minor way, but with strong financial implications.**  Remember, in the previous case, the cost of a false positive was $500.  Let's say our model predicts B is going to occur on a particular machine, but this is incorrect.  Actually, issue A was going to occur.  We now have spent $500 for the unnecessary work done to fix the issue B that was not going to occur and $500 to reactively fix issue A when it occurs in the future.  So the cost of a false positive is $1000 in this case, not just $500.

We can look at the impact of this by performing a similar simulation to what was was one before.  One of the benefits of doing this via simulation (as opposed to using real service data) is being able to see which of the false positives was **unnecessary work** (we predicted an issue when no issue was going to occur) and **incorrect work** (we predicted the wrong issue was going to occur). This allows us to split out which false positives cost $500 and which cost $1000 and we can better see the cost impact.

As a reminder, the maximum potential cost savings if all the Alarms are correct is $25K.

| Number of Alarms | Percentage Label Noise | Unnecessary Work | Incorrect Work | Financial Cost of False Alarms | Final Cost Savings |
| ---------------- | ---------------------- | ---------------- | -------------- | ------------------------------ | ------------------ |
| 100              | 0                      | 5                | 8              | $10500                         | $14500             |
| 100              | 5                      | 5                | 8              | $10500                         | $14500             |
| 100              | 10                     | 5                | 14             | $16500                         | $8500              |
| 100              | 20                     | 6                | 15             | $18000                         | $7000              |

In this scenario, you can see over 50% of the potential cost savings is gone with just 10% label noise.  Also note the "unnecessary work" is fairly stable.  Most of the loses are from the "incorrect work" which is when the model predicts the wrong issue is going to occur, resulting in double the work.

The other interesting observation is how high the no noise baseline is compared to the previous model that only predicted a single issue.  When predicting two issues, $10K of value is erased even when the labels are all correct.  If you had this information before you built a model and were deciding if it was worth the effort to develop the model, would you do it?

Keep in mind what you are seeing in this table is partly an artifact of the model and methodology I used.  It's possible to use different models and tune them so the effect of label noise is different (e.g. reducing the number of Alarms and increasing the accuracy of each Alarm, but at the cost of more missing issues/more false negatives).  The trends you are seeing in this table are illustrative of a particular methodology, and are not the gold standard for what you will see with your models.  You should validate this with your own data.
# Answers to Possible Questions

**What's a realistic percentage for how much label noise I will see in my data?**

I've seen so much variance in data quality that it's very difficult to estimate this.  It's possible for a single service dataset to have very different noise percentages for different issues or labels, so it's challenging to provide a single number.

In my experience, 10% label noise is usually very good and very rare.  It's usually much worse. 

**If label noise is so pervasive, how can any predictive maintenance projects succeed?**

The goal of this article was not to dissuade your efforts around predictive maintenance, but to demonstrate how you need to think about goals as a tradeoff, not an absolute.  Instead of deciding that proactive maintenance is going to hit a particular cost savings target and building as many models as possible to hopefully hit that target, think about doing a rigorous analysis of your data and service costs to better understand how much you can really save. Figure out how many good alarms you need and if that is achievable with the data you have.  Try to determine the best and worst case for how many bad alarms you might get, and if the financial impact of those will still allow your program to make sense financially.  If your numbers still look good after doing that analysis, do a quick pilot project or run some experiments to see if you can validate those cost savings in the real world.

I've talked about this in more detail in another article where I wrote about creating a [success criteria](https://ujkam.com/posts/the-anatomy-of-a-predictive-maintenance-project/#bad-success-criteria-will-kill-your-program) for predictive maintenance projects.

**Does a proactive service visit really cost the same as a reactive service visit?**

One of the assumptions I made was a false positive costing the same as a reactive service visit.  I made this assumption to make the math easier, but I don't agree with it.  I think the cost of a false alarm should be higher than the cost of doing the exact same service reactively because of the negative effect it has on your service operations.

Consider the logistics of a service organization.  At least in my experience, service technicians and other service personnel aren't sitting around waiting for things to do. There is an opportunity cost to having them do unnecessary work.  If a company has limited personnel and they are being assigned to a false positive, that probably means somebody else with a genuine issue is waiting for service technicians to become available.  The technician could also be using up a limited stock of parts during an unnecessary service, resulting in a customer with a genuine issue waiting for a fix due a lack of parts.  This opportunity cost needs to be accounted for somehow, and the cost of a false positive needs to be more than the cost of the same reactive service.  I leave it to you to decide how much more this should be based on the service challenges you see in your company.

**Instead of using one model to predict two issues, can I use two models, each of which predicts a single issue?  Will that reduce the number of false positives?**

The answer to the first question is yes, you can build models that only predict a single issue.  Many organizations do exactly this, as it can be easier to define the use case and also evaluate and track the quality of the Alarms this way.

The answer to the second question is less clear.  Something to keep in mind is even if your model is built to predict only one issue, your machine and service data may not clearly separate out the issue you are predicting from other issues occurring at the same time.  To better understand this, let's go back to the example I gave about a cracked and leaking water pipe.

To detect a water leak, one of the ways to measure this is to track water pressures.  However, these measurements are not likely to measure only the water pressure of a single hose, but the water pressure throughout your system.  So if other issues are affecting the water pressure, those will influence the water pressure data being collected.  This, in turn, will have an effect on your predictions since you can't cleanly isolate other issues from the issue you are trying to predict.  So it's possible the financial costs of the false alarms from two models predicting separate issues will be the same as using a single model.  You just have to try it and see which works better.

# Take Aways

If nothing else, there are two major things you should take away from this.  The first is false positives, or incorrect Alarms, can quickly eliminate all the business value of your predictive maintenance efforts.  Don't let statements like "Our model is 85% accurate" trick you into thinking that 85% is giving you five times more value than the 15% is taking away.  Do an honest assessment to see if you are really getting the value you need out of a predictive model.

The second take away is about the value of improving data quality.  Data People frequently complain about underinvestment in data quality and how other people don't really understand the problems caused by poor data quality.  I think part of the reason this happens is because most people don't look at raw data, they look at summary KPIs.  And then unconsciously assume that if they can drive major business decisions with their KPIs, their data quality is good enough for everything else.

An example of this can be seen in the way people sometimes evaluate how machine issues are driving overall service costs.  One way to approach this is using the pareto principle and focusing on the issues driving the most costs.  You can take one year of data and add up the number of times each issue occurred, and multiply that by the financial costs of each issue.  In this way, you'll see which issues are costing you the most annually.  Even If 20% of the issue labels are incorrect, the ranking of the individual items in the pareto might be a bit different, but the big cost drivers will probably still be the big cost drivers, and the small issues are less of a cost concern when compared to the big issues.  So even with 20% label noise, your pareto breakdown is still correct.  So you might assume the data is good enough for anything you want to do with it.

As we've seen, that 20% matters a lot when it comes to getting the business value you expected from predictive models.  In many cases, fixing those data quality issues isn't just about hiring a person to focus on cleaning up data, but about revamping the entire process and software interface used to collect data. 

As a manager, it can be tiresome to listen to Data Scientists complain about data quality, especially when the quality seems fine for everybody else.  Just remember, what they might be trying to tell you is that with your current data, your goal of saving X million dollars using predictive maintenance is just an unattainable dream.  If you aren't prepared invest in improving your data quality, it's better if you know that today instead of waiting years for the program to fall flat on it's face.












 














































<!--more-->
