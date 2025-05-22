+++
title = 'What is Business Domain Experience?'
date = 2025-05-15T00:00:00-04:00
draft = false
mermaid = false
+++

As a Data Scientist, one popular piece of career advice is that you should have business domain experience (or expertise) and not just technical skills.  But what is "domain experience" exactly, and why does it help differentiate one person from another?  In this post I'll talk about the different kinds of domain expertise I've seen at all the different companies where I've worked.

<!--more-->

When writing these articles, I try not to make too many assumptions about the reader.  Consequently, I've tried to explicitly define some terms and concepts I use to explain the different types of expertise.  Readers with career experience may find these explanations superfluous, but I think it will help clarify what exactly it is I'm referring to when I use terms like "the business" and "industry."  Students, people earlier in their careers, and people buried near the bottom of a 8 layer org chart with limited exposure to the larger business, might find the explicitness useful.
# Defining "The Business"

In this article, I use terms like "the business" and "business people."  To reduce confusion, I think it's worth the effort to clarify what I mean by the term "business."

Imagine you have a friend who sells different colored T-shirts.  The entire company is this one person, and their current business model is ordering T-shirts in bulk from a supplier and selling them to customers via a simple website.  One day they contact you (a Data person) and say their business is growing, they're having inventory issues, and they need a dashboard to help them better track sales and also to help identify what color and size combinations they should be ordering and how often they should be ordering them.  They've also noticed certain colors get purchased more frequently before local events, and they need help with tracking those events.

In this example,  all the things your friend does including sales, ordering, inventory, packaging and shipping, etc. is "the business."  And what they've asked you to do is to support the business by building tools to help them make better business decisions.

One important point to recognize is the sales data from this business does not exist in a vacuum.  It's a byproduct of the decisions and the actions of the business owner. To truly understand the data, you'll have to talk to your friend and ask questions.  For example, let's say normally they sell a shirt for $10, but there's one customer who purchased a lot of them for $9.  This seems like it might be from a coupon or sale, but the only way to figure it out is to ask your friend.

As T-shirt sales grow, your friend is not able to keep up with running the different activities required for the business.  So they hire an additional person to handle sales and customer service and another person to run shipping and returns.  The new sales person wants to sell new colors and also trial using discounts to boost sales of older inventory.  They want you to build analytics to help them understand the impact of both these changes.  The shipping and returns person also wants a dashboard so they can more easily track returns and understand if there are some larger issues being missed (e.g. some issue with a particular color).

To provide useful analytics to these new people, you'll need to ask them what they want.  The sales person might say what they really want is to understand if discounts have too strong of an effect in pushing people to buy older inventory, resulting in lower sales of the full priced shirts.  The customer service person might ask for something you didn't even realize they cared about.

**You should realize that you, as a Data person, are not making business decisions.  Those decisions are made by people in "business" roles who support a particular business function (e.g. sales).  Your job is to support them in making those decisions. The data is not your data and nothing you do as a Data person is generating data.  This company's data is a byproduct of sales activity, inventory management, and customer service.**

As the company grows, these single person departments become multiple person departments, and they add new departments like accounting and HR.  Your friend the CEO decides to print T-shirts as well, so they hire dedicated staff to manage and run printing operations.  At some point, the company is so big some departments don't clearly understand exactly what it is the other departments do, but they all generate data as part of their operations and activities.

When the company was run by only your friend, their business model was simple enough for you to understand the data on your own.  But as the company grew, you needed more insight from each department to understand the data, and to understand what a useful analysis would be for each business person/unit.  **It's important to recognize just having data is of limited value.  Understanding how business people extract value from data, and use it to make decisions, defines the value of the data.**

Now that we have a better understanding of what I mean by "business", and also what separates the activities of a a data person from the activities of somebody in a business role, let's talk about domain knowledge.

## Domain versus Business Knowledge

One other distinction worth talking about explicitly is the difference between **domain knowledge** and **business knowledge.**  These terms are used interchangeably in a lot of contexts, but it can be confusing when talking about data roles because data people have skills intersecting both of these.

I'll use something from my professional life as an example.  I have a lot of experience building data pipelines, models, and dashboards based on files and datasets from manufacturing and industrial machines.  This might seem like one skill, but it's actually two.  I have technical domain knowledge about ingesting and processing machine data formats, but I also have the business industry knowledge to understand the actual data and how it's used by people on the business side of the table.

Domain expertise can exist separately from business knowledge.  It's entirely possible to be an expert at ingesting raw manufacturing datasets, but not really understand the business operations generating the data, or how the business uses this data to make decisions. The opposite is also true.  You can be an expert on a type of machine and have the knowledge to make high value decisions based on the machine data, but you don't have the software domain skills to use the data in it's raw form.  You need somebody (e.g. a Data Engineer) to transform it into a format you find usable.

One of the big differences between Data Scientists/Analyst and Software Engineers is domain versus business expertise.  Software Engineers are domain experts in software. They typically also have additional subdomain expertise (e.g. image processing), and they can have highly specific industry expertise (e.g. processing 3D scan data from medical devices), while having limited knowledge of how business people run their operations and make decisions.  In contrast, Data Scientists/Analysts need to have knowledge of the business because what they build needs to be useful to the people who make decisions based on that data.

# The Different Kinds of Domain Knowledge

Speaking broadly, let's split domain knowledge into four categories. 1) technical skills, 2) industry expertise, 3) internal company knowledge, and 4) business process knowledge.  It's important to recognize the expertise people develop in their careers are an intersection of at least two of these categories.  Once you start a job, technical skills do not exist without the context of the industry the job is in, even if that industry is the software industry.  Internal company knowledge and business process expertise feed each other, and they are defined by the industry they are being formed within.  Technical skills and business process knowledge go hand-in-hand, as you can imagine the development, production, and deployment requirements of medical software are very different than a photo viewing app.

These categories are artificially separated for the sake of this article.  If you are reading about one of them and you feel like the examples I've provided fits into more than one category, you're probably correct.
# Technical Skills

Technical skills are the basic "hard skills" you need for any Data Science role.  There's a lot of variance in what these skills are because every industry is different, but these are the fundamental skills required to be an individual contributor. Examples of technical skills are SQL, basic statistics, and a programming language (R/Python).

There are also industry specific technical skills.  For example, if you are working in marketing trying to understand the impact of your advertising spending, there are specific types of models (e.g. Bayesian Regression and Casual Modeling) you need to know.  If you work with sensor data, it's useful to know about signal processing and methods to deal with sensor noise.

To some degree, you can learn industry specific technical skills from a textbook or course, but there is a point at which only real world experience can teach you how to properly apply these skills.  And as you get more expertise with applying your knowledge to real use cases, the faster and more efficient you'll become at your job.  

Here is an example of where real-world experience enhances your technical skills.  Time Series Forecasting models (e.g. ARIMA, GARCH) have been around for a long time and there is plenty of material to learn about them and when and how to use them.  Picking the right parameters is a bit of art form, and one tactic inexperienced people use is to simply brute-force all the parameters and pick the model with the smallest errors.  However, there are sometimes industry or team specific guidelines for certain parameters to make things easier.  Someone I know who worked in financial modeling told me people in finance typically set one particular parameter of the model to 1 because it doesn't make much sense to set it to any other value when you try to interpret the model results.  This made a lot of sense when it was explained to me, but I wouldn't have even thought of it on my own.

Industry specific technical knowledge isn't limited to statistical modeling and machine learning.  Having this type of domain experience can also simplify and accelerate how you think about data schemas and data engineering.  An example of this is charts.  In the real world, the types of charts people use can be very domain and even industry specific. This means you might find yourself building different versions of the same chart a lot of times.  For example, if your users always want comparison charts (e.g. give us weekly sales of product A vs B vs C), you'll quickly discover BI tools work better with tall data (having a single product column and a single sales column) versus using wide data (having a separate sales column for every product), and create your schemas and pipelines accordingly.  Based on what users expect, you'll also figure out what calculations should be done in the database, which should be done in the data pipeline, and which can be done in the BI tool.
# Industry Expertise

Industry Expertise comes from understanding the industry you are in.  If you are in a technical individual contributor role, what you learn about your industry will be specific to the type of business function you support.  For example, if you work in a sales organization, you're going to learn about how the sales cycle works and what really drives sales.  And even in sales, you're going to learn very different things if the product your company sells is a high-touch (e.g. selling to government) product with a long sales cycle, or a low-touch SaaS product (e.g. selling a $10/month API based product to software developers).  

Industry Expertise has both technical and non-technical components.  To better illustrate the difference, here are two examples.

To test pricing and the effect of product placement on store shelves, grocery stores and product manufacturers (e.g. a company making laundry detergents) run experiments in simulated and actual stores.  Analytics people have the technical skills to analyze the data from these experiments. But to run the actual experiment you need people who understand the grocery business and the methodologies and processes of running these tests in the actual physical world.

When you first start working in this type of job, you might have the technical (software + statistics) skills to process and analyze the data in a general sense, but you won't have the industry domain knowledge about the processes generating that data.  The more time you spend in this domain, the more exposure you'll have to the non technical aspects.  You'll better understand what the non-technical people do, what their pain points are, and what they need to do their job more effectively.  This exposure will allow you to better adjust your technical work so what you create is consumable and useful to the business people.

For the second example, I'll take my own experience.  I've worked a lot with teams who operate and service industrial and commercial machines.  From a technical standpoint, I've learned a lot about machine and service data and the types of methods and models used to analyze this data.  From an industry perspective, I've learned a lot about the KPIs and insights business users are looking for.

These insights and KPIs also vary between service teams.  Some machines run in remote areas or underground, and there's no real way to perform physical maintenance on them. So the goal is to avoid a catastrophic failure because it's very difficult to replace a dead machine.  The models used and KPIs for this scenario might be very different from machines which run in a chemical processing facility, where it's easier to access the machine.  In this latter case, using predictive maintenance to maximize machine runtime is more of a concern than a catastrophic loss. 

Something I've found interesting is it's entirely possible to have an expert level understanding of important metrics while not really knowing how to recreate those metrics from raw data.  The opposite is also true.  There are people who are experts on the raw data and transforming it into KPIs, but they don't know how to provide the right context to those KPIs to make a business decision.  I've been in the latter position many times, and I think Data Engineers are generally the same way.  I mostly write this paragraph so people realize industry "expertise" isn't a binary category (e.g. you are an expert or you aren't), but it's sometimes a seemingly contradictory sliding scale.  You can be considered an expert at something without knowing every level of it.

For Data Scientists and other Data roles, one challenge is how much industry knowledge you learn is highly dependent on how much exposure you have to the business side and customers.  It's entirely possible to be buried so far down the org chart  you don't understand your industry very well at all.  I've seen this happen more frequently to people who are in an Engineering (e.g. Machine Learning Engineers) org, where they are managed by a user story and subtasks, as opposed to a role where your projects are primarily led by people on the business side. 
# Internal Company Knowledge 

After you've been in a job for a while, you'll start acquiring internal knowledge.  It's not always easy to see this, but internal knowledge is a superpower.  There's this concept of a 10X engineer, and I think it's easy to assume a 10X person has to be a genius like Ramanujan, Mozart, Carmack, etc, therefore you can't be a 10X person.  But many 10X people become that way after mastering internal knowledge and systems, or having a superior understanding about the way different parts of internal systems overlap.  If your specialized knowledge of 3 internal systems means you can identify an issue in an hour, whereas it would take most other people days, you're a 10X person.

In my view, internal company knowledge breaks down into four broad categories a) knowing people b) knowing where to find things c) knowing your company's data and technical systems, and d) knowing what drives the business and how to measure it.  
## Knowing People 

Knowing people is really about knowing the right person to ask when you need help with something. That "something" could be data access, help with understanding how some business goal is measured, technical expertise, or even knowing the person who seems to always know the right person/team to ask.

The longer you are at a job, the more the "help" you need from others shifts from a concrete thing (e.g. who can give me access to this), to soft-skill topics like "How can I motivate you to prioritize this fix."  Knowing the right people to talk to is really important to doing your job effectively.

## Knowing Where to Find Things

"Things" in companies are documentation, wikis, SharePoint sites, customer service tickets, manuals, release notes, etc.  LLMs and techniques like RAG will make this easier, but internal information is usually spread out all over the place.  And every org in every company does things differently, so it takes time to get a good mental model of where answers might be when you have a question. 

Knowing the right people is part of this because the right person can quickly tell you where to find something.  I have a very distinct memory from one my jobs where I was fumbling around trying to figure out how to integrate two pieces of software, and somebody I was working with said "Well I know at some point they announced our product supports that integration, so it's probably in this manual" and sure enough, it was fully documented in that exact manual.  My prognosis on what what we were trying to do went from impossible to easy in about 2 minutes. 
## Knowing Your Data Systems

Knowing your company's data is a really broad topic, and it's going to differ from company to company, and every from org to org (e.g. the data systems used by the Sales team are going to be very different from what the Engineering team might use)
Examples of knowing your data includes
- Knowing the right database and tables to use for the questions you are trying to answer
	- Knowing the right columns to use in an individual table
	- Knowing the right tables to join together, and the right way to join them
- Knowing how to join tables across databases so they align properly
- Knowing the right data tools to use for whatever task you are trying to do
- Knowing the recommend way to connect together all the data tools you are trying to use.
	- This assumes there are internal best practices around this.  There are many places where the data infrastructure is hodgepodge of tools and people just pick the ones they prefer.  It can lead to very brittle data stack.
- Knowing how to deal with all the brittle and weak links in the companies data stack.  
	- For example, you might learn a certain database is extremely slow during working hours, so it's better to export the data in the middle of the night and then use the exports during the day.

When you start any data role, it can be quite unsettling and overwhelming to open a database and see hundreds of tables and columns with completely undecipherable names.  This compounds if you have multiple databases with different schemas, and you have no idea how any of this can possibly fit together.  Over time you'll figure all this out and one day you'll be the expert people go to when they are trying to find the right data to answer a question.  

One quirk with data expertise comes from how common it is nowadays for people to change jobs, even if it's an internal change.  It's very possible one year after you start a job, you are already perceived as an "expert" for a particular set of data, even if you still feel like a novice.  It's important to recognize when you are an expert compared to your peers and embrace that.  You need to be able to understand your value.

In a technical data role, knowing your data usually means you know about your technical infrastructure as well.  This is because the technical work you do (e.g. write data pipelines in PySpark) is usually intertwined with the technical infrastructure you use to perform that work.  So unless your job is 100% writing SQL in a web browser, learning about your data systems means you learn about the surrounding technical infrastructure as well. 
## Knowing the Business 

The last category is knowing the business.  This is a very fuzzy concept because "business" knowledge overlaps with industry knowledge, and internal knowledge is knowledge about the particular business your work for.  For example, the KPIs and practices used by a particular business function (e.g. Manufacturing) are typically based on industry standard KPIs and practices.  

Knowing the business is really about understanding what motivates the business people you work with, what they are looking for, and what drives the business.  To understand that well, it's very helpful to understand business processes.  But before we get into business processes, let's first talk about how technical and data knowledge are relevant here. 

When business people ask a Data Scientist or another Data person for help, there's usually some metric or KPI they are trying to effect or improve.  When you work with a business person to understand how that metric/KPI can be moved, you'll discover what business activities move the metric and how these activities move it. And as you start understanding those activities, you'll understand the business you are working in.

This business understanding is really important because it allows you to discover what business activities you can influence.  For example, maybe a KPI is based on the combination of four separate, but intertwined, business activities.  Your business partner says one of those activities cannot be changed, and you discover you don't have the data to really measure or influence one of the other activities.  These leaves you with the other two, and you now know where to focus your efforts.

To better illustrate the idea of only focusing on business activities where you can actually make an impact, here are two examples.  First, let's look at the commercial service industry.  This is where some equipment breaks and somebody goes out to fix it.  In theory, the various activities (e.g. speaking with a customer about an issue, assigning somebody to fix it, ordering parts, somebody actually going out to fix it) are common across the service industry.  But individual companies will make decisions about how they go about these activities based on their own constraints.  For example, your service department might only accept service calls with a minimum of 4 hours of work.  This could mean you bundle together a lot of small (e.g. 30 minute fixes) projects into one customer visit, but you won't go to a customer site for a single small project.  If you're developing models, you might realize it's not worth the effort to develop certain types of models (e.g. predicting a small fix) because the service department will refuse to act on them.  If you know this before you build the model and you focus your work accordingly, that's an example of using business knowledge to make better decisions.

Another example is marketing activities.  Marketing, in a general sense, has certain activities performed by all companies.  However, different marketing departments can choose to outsource certain parts of what they are doing, and this will influence how much analytics built by internal teams can really influence and drive decisions.  I used to work for a large company with many brands.  Each brand had it's own business unit, and the one I worked for had around 5% of the total sales of the company.  This directly impacted the size of our marketing department, and what activities could be done by our team versus asking for support from another brand or giving it to an external vendor.  One of these activities was Marketing Mix Modeling (MMM).  In theory, our small Data Science team could have tried to do it.  But it was more pragmatic to give this to an external vendor so we could focus on topics with a shorter time impact.

Let's tie this idea of business activities back to KPIs and metrics.  In the service example, the 4 hour minimum is not a completely arbitrary decision. There is probably a metric to calculate the profitability of service visits, and service visits of less than 4 hours drives that metric below a target.  This profitability metric may be in contrast to another customer satisfaction metric, which is negatively affected by this 4 hours minimum requirement , but not enough to change 4 to another number.  As a Data Scientist, you should understand the activities which drive these metrics. The business people you support are evaluated based on these metrics, so it's your job to work with them to identify how your work can improve what they do.

In the marketing mix example, I said it was more pragmatic to give it an external vendor.  Let me explain why.  When people make expensive purchasing decisions (e.g. a car, kitchen appliances, an expensive purse), brand perception and recall matter.  Even if you have a great product, people will be hesitant to buy it if they don't know or trust your brand.  When they are cross-shopping and thinking of what brands to check, people may not even remember your brand or product exists.

There are industry standard KPIs for brand perception. ([Google search for Brand KPIs)](https://www.google.com/search?client=firefox-b-1-d&q=brand+kpis))
Marketing departments strive to improve their brand's ratings on these KPIs, but simply marketing is not enough. Brand perception is built over time, and it requires reliable products with the perception of high quality.  Customer service is important as well.  If your brand has no reputation or a poor one, it takes years of executing well on all fronts to improve the brand KPIs.  Brand perception has a direct effect on Marketing Mix Modeling.  If nobody trusts your brand, throwing money at certain advertising channels isn't going to meaningfully improve sales or brand KPIs.  You can't optimize your way out of a mediocre reputation.  

A small Marketing org with a small Data Science team is unlikely to be able to move those brand KPIs.  It's a long term effort by the entire company.  Even though MMM is very interesting and it's frustrating to give it to an external team, there are other marketing KPIs with a shorter time horizon where you can demonstrate improvement.  It's only by understanding the company's marketing business that you, as a Data Scientist, are able realize you can have more of an impact by focusing on other problems, rather than trying to optimize advertising spend.

Every company and organizational unit is going to have different business goals, KPIs, and metrics.  Sometimes the metrics between orgs in the same company are contradictory!  As a data person, knowing what drives these measures gives you a unique insight into what the business teams can and cannot do.  This, in turn, gives you you the small superpower of being able to quickly diagnose what business people really want (what needle are they trying to move?) out of a use case, and determine the feasibility.

# Business Process Knowledge

Any sort of business activities (e.g. marketing, sales, engineering, manufacturing) will be composed of many individual steps.  Established companies are going to have business processes for performing these steps, and for coordinating between different teams and organizations. As a Data Scientist, you're going to have to follow these processes.  Depending on how your Data Science function operates with other teams, you'll probably also be involved in improving existing processes or creating new processes.  Having a process helps set expectations and timelines, it protects the organization from focusing on low value work, and it reduces the friction between the Data Science team and the other teams involved in projects.

In my view, there are two broad categories of business processes.  The first type is based on the organizational structure of the company, and it's largely internal knowledge.  The second type is organized around the industry forces the process is operating in.  Many processes are a hybrid of both.

A common example of the first type is getting approval for a new laptop or monitor.  The complexity of this simple sounding action is going to vary a lot based on company guidelines and processes.  Another example is choosing between project management frameworks such as Scrum, Kanban, Scaled Agile, etc.  In theory people pick the one best suited for the product they are developing.  In practice, internal organizational structures and boundaries define which one is used, dictating a lot of how you work.  

An example of the second type is when a company is making products for the medical industry.  Typically, there are regulatory requirements influencing the process for releasing models to production.  The closer the model is to influencing decisions made about patients, the more the regulatory process is going to drive the model testing, validation, and release process.  Considerations of legal consequences will influence processes as well. 

The type of customers being sold to also influences business processes.  If you sell a software product to other businesses, your process around testing and deploying models is going to be different than if the company you work for has a "free" software product primarily funded by advertising.  Processes will be different between a hardware and software product as well, especially if you are comparing hardware that should never get things wrong (e.g. controls on a airplane) versus a basic physical fitness tracker.

Speaking more specifically about Data Science, a concrete example of a business process is how you vet and triage requests for predictive models from the business.  If you are a small data team and you have a lot of requests for predictive models from different teams, it's useful to have an established process for how the business teams can propose projects and provide a financial justification for their requests, and also define what they need to provide in terms of support (e.g. they need to provide an business domain expert).  Having this type of process ensures everybody, inside and outside the Data Science team, is willing to invest their time and money into a project.  It helps to keep things fair.

Once a request passes this approval process and a Data Scientist starts working on it, it's useful to have some process to continuously validate the feasibility of the project and have criteria to pause it.  This is so you don't waste time trying to get something to production, when it was clear 3 weeks in you didn't have the data to support it.  

In addition to processes about how you work with the business, there are technical processes and best practices as well.  These will dictate how you develop, test, deploy, and release your code.  This includes things like how to write documentation, who has to sign off on documentation, etc.

Let's wrap up this section by looking at processes from a day-to-day job perspective.  At a high level, there are three types of activities you will do.  You will create business processes, you will enforce business processes, or will follow business processes.  Since many Data Science teams are part of supporting a larger business activity, they are always doing the last one.  In smaller teams, you should be doing the other two activities as well.  I'll talk more about this later in the general career advice section.

# The Impact on Your Career 

## The Intersection of Technical Skills and Industry Knowledge

I don't think I need to explain why having technical skills is important to being successful in a Data Science role, or Data roles in general.  So instead of having a separate section just on technical skills, let's talk about the intersection with industry knowledge.

One thing I've seen a lot is people refusing to learn statistical or ML techniques specific to the industry they're working in.  For example, people working on forecasting problems, but refusing to learn time series models (e.g. ARIMA) because they can get "acceptable" results from a more general purpose ML or Deep Learning model.

I do understand in many Data Science jobs, nobody cares how you did it, they just want you to do it.  So it's easy to feel like you have no time nor motivation to use a model which requires more manual tuning.  But I would really encourage you to learn about the industry specific techniques, because it'll teach you a lot about how these problems are formulated and how people think about them.  It'll also give you a better vocabulary to use when communicating to non Data Scientists about topics like risk and how much you can trust your models.

I'd also encourage you to think this way about industry specific technology products that you have to interact with, but not necessarily use directly.  An example of this is products from SAP.  SAP products are used a lot in manufacturing and other service oriented environments.  As a Data Scientist, you might never query an SAP system directly, but might be ingesting SAP data into your project from a data warehouse.  This is true of many other proprietary and open-source products as well.  They are part of the software stack somewhere that generates data you use, but you never interact with them.

You don't have to learn to use or administer those products, or do any formal training. But I'd really recommend that if you have the opportunity, you have somebody walk you through the process of how that product is used as part of their jobs. This will give you a lot of context in understanding how people work and the business model they are working in.  Technology solutions are a representation of the business activities people are performing.

Since I already mentioned SAP, I'll use it as an example.  I've worked with a lot of service data.  Like most data, it has a lot of quirks, but I was able to work with this data for years without really understanding how it was generated.  Then, during one particularly tough project, I reached out to someone and asked if they could show me the SAP ERP UI they used every day.  They did me one better, and actually walked me through exactly what happens when a customer calls in an issue, when a ticket is created, and showed me the information for an existing ticket.

It was absolutely eye opening.  Not only was I now able to understand where all the  quirks I saw were coming from, I better understood the whole process of what the service folks did in their job, and how the data reflected that.  I also got a lot of insight into how technicians looked at data, and how what our team gave them (e.g. predictions and documentation) could be better put into their language, and not just ours.

Another major benefit was allowing me to get a better idea of the service industry in general.  Industry specific software systems strive to be an approximation of what people in that industry are doing.  Understanding the user flow in these systems provides insight into what people really do in their jobs.  Sometimes it's a very poor approximation, but purely from an educational standpoint, it's better than nothing.

Every industry or function has technology like this.  Salesforce, Workday, Servicenow, Healthcare EMR systems, JIRA, physical control systems, etc, are all examples.  Regardless of if people love or hate those systems, I encourage you to try and find the interest and mental justification to understand how people use them.

## How Useful is Internal Knowledge? 

Internal knowledge exists in two simultaneous contradictory states.  On one hand, strong internal knowledge makes you an expert, and you are perceived as valuable.  At the same time internal knowledge can feel fairly useless when you have to wrestle with seemingly arbitrary things on a daily basis.  I used to work at a place with a lot of different tables across different databases, and the column names were not always informative.  Documentation (if it existed at all) was in different places, and sometimes outdated.  Anybody using this data had to invest a large amount of time and mental space to understand it.

There were frustrating days when I really questioned how much of this knowledge was useful to my career.  If I changed companies, or even just changed my role into a different org, a significant portion of this knowledge would be of no value.  At the same time, one day my manager messaged me saying something had gone wrong in production and the cause was unclear.  Could I help fix it?  And sure enough, my knowledge of the system meant we were able to quickly find the interaction between the data and the code causing the issue.  

When you're trying to understand the value of internal knowledge, it's important to abstract up from the particular thing you are dealing with and figure out what the larger, more general, skill is.  Instead of of seeing your data infrastructure as a huge mess, and perceiving your expertise in that mess as useless knowledge, understand data infrastructure in general is a huge mess.  And the more established the company, the bigger the mess is.  Learning to have the patience and curiosity to navigate and untangle that mess is a valuable and transferable skill.  And to learn the more general transferable skills, you need the specific experiences you have had untangling your particular data mess.

This is true for non-technical internal knowledge as well.  Being able to recognize when it's better to reach out for help instead of continuing to try to figure something out yourself is a skill.  When you look at types of people, some people are always willing to help.  Others will only help if they perceive a problem as worth their time.  Some people need a managerial intervention before they even acknowledge you exist.  Every company has a different culture.  But unless you know the people and places to reach out to, and how and when they can help, you'll never build the internal compass to know when to do what.  Allow yourself to recognize the value of non-technical internal knowledge, and remember it's takes time to learn it. 

## Learn as Much Business Domain Knowledge as Possible

I hope I was able to clearly illustrate the value of learning about the business in the dedicated section under Internal Company Knowledge.  Instead of reiterating this point, let me talk about one of my regrets from a career growth perspective.

When I was in consulting, I met with lots of people in the business.  But because I was normally heads down and under time constraints, I usually only asked enough about the business side of things to get my work done.  It's like I had access to an entire encyclopedia, but I was only focused on the pages related to my work.

As you progress in your career, knowing the business becomes increasingly important.  If you look at technical and non-technical job ladders, interacting with people and making sure your team is pointing in the right direction becomes more important than solving a very specific technical problem.  You need to be able to understand the business to know when you are pointing in the right direction.

If you don't understand the business you are working in, search engines and LLMs can help you find an astonishing amount of information about your industry, and even your specific business activities and operations.  I just typed in "How is an oil well drilled?" and "What data is collected when an oil well is drilled?" into an LLM.  The response was fantastic, and gave me a detailed overview of drilling and drilling data.  I'm not saying you should trust these answers, but it gives you a great starting point to have a conversation with a business person without having to start from zero.  Use what you find to bootstrap your conversations and learn about the particular company and people you support. 
## Surviving Business Processes

A project manager I liked working with once interrupted a meeting by saying "I just wanted to say this format wasn't my idea, and I proposed something else.  But it's what we have to work with."  That sums up a lot of the business processes you'll see as a Data Scientist.  Learn it, ask questions if needed, and try to follow the process without getting too frustrated.

That being said, don't automatically become a victim of business processes.  The world of Data Science is very new, and many people don't really have the knowledge to realize when some part of a process just isn't working well for a Data Science project.  This is because they are applying incompatible ideas from other domains to data processes. 

You may not like to do this, but sometimes to fix a part of the process, you have understand all the parts of the process and where they came from.  This allows you to get the right context, and then figure out the best way to explain how broken it is and demonstrate possible ways to fix it.  Don't just keeping complaining about how things make no sense.  Asking questions and constructively pointing out problems will help good people realize the process no longer serves the intended purpose. 

It's astonishing how many times the answer to "Why do things this way?" turns out to be nobody seems to know why, and the person who came up with this process left years ago.  Those answers mean there a possibility to change things because nobody has a strong reason to block you outside of "We've always done it this way."  It also means if you have to make a change first and ask for forgiveness later, nobody really has a strong reason to push back.

In the previous section about this topic, I had mentioned in addition to following processes you would be creating and enforcing processes as well.  From what I've seen, a Data Science team is unlikely to create a process dictating how cross functional teams work.  This is usually reserved for someone with a managerial title (e.g. project/program/director).  What you can and should do is modify or create sub-processes to help your data science work.  Create a process, template, or rubric to validate new and ongoing projects, and for evaluating when a model is good enough to go into production.  The process should have steps for your business partners so it forces them to do something and have some accountability.  

While these processes can slow you down, it also allows you to enforce them when you need to.  There are no shortage of project ideas, and there are people who will constantly push for you to work on your pet project, even if you can't validate the business value.  Having a process means you can say "Based on this process, we can't move forward" and point to your process as a more objective measure for saying no.  

Like many others, I find many business processes frustrating and sometimes nonsensical.  This is especially true for processes created outside of your team.  I do, however, realize they exist to to try and prevent bigger problems.  I think it's worth the effort to come to terms with why business processes are needed, and to develop a proper understanding of the processes directly effecting your job.  People will take you more seriously if you actually understand the processes you are a part of.

# One Last Thing

To finish up this article, I just want to briefly talk about processes internal to a Data Science team.  At least in my view, these aren't rigid processes as much as they are best practices and checklists.  If you've got a healthy team that works well together, you trust each other to do the right thing and learn from mistakes.  If a checklist or best practice needs to be updated, you discuss it and change it without interference from outside the team.  However, if you've got team issues or a problematic teammate, creating rigid processes can become a dysfunctional way to deal with it, since it forces people to do things in a certain way.  It also allows you to point the finger if someone doesn't follow the "process."  Personally, I feel like these things are people management issues, not really process issues.  That's the reason I haven't included these types of topics in the business process section.


























