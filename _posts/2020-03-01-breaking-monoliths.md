---
layout: posts
title:  "Breaking a monolith"
date:   2020-03-01 10:47:34 -0600
categories: monolith
---

QuickBooks launched in 1993 as a desktop solution for DIY accounting, it was an immediate success and within a few years business started doubling revenue year over year. Application users quickly realized an inefficiency, users were forced to enter the data twice; first in the e-commerce site and after the invoice was paid it had to be entered again into the Accounting system. This is inefficient and error prone.

Enterprising third party developers realized this opportunity and stared demanding an open ecosystem to enable integration between e-commerce sites and QuickBooks. A barebones developer portal was launched to guide developers on how to build integrations. Overtime, the portal evolved as Inuit started launching APIs for integrations. As the developer base grew several experiments were launched to monetize or improve billing. Each improvement  bloated developer portal's Codebase. Resulting in a application that was expensive to maintain and add new features to. Users felt this inefficiency and gave the site an NPS score of -17.

An effort was launched to transform the application with breaking the monolithic application code, moving the application to AWS and  upgrading the stack to React, Java and Aurora. A  team of 6 engineers, 1 PM and 1 designer came together launch the new portal. In the  new version code size reduced by 50%, cycle time for a change went down from weeks to minutes. The team started responding to user voice on same day. This resulted in a developer NPS of +38 within six months of launching the site. Following are learning from this journey. 

### Team

A team that works well together is critical to the success of the project, with this rest of the pieces fall into place seamlessly. A good team has diverse members, they trust each other, team members are passionate about their craft and hungry to learn. 

As a team leader you are responsible for

* building a team of owners. A good product needs support from product managers, designers, support engineers and only a team acting as owners can bring this group together effectively.
* Communicate clear goals and have a plan ready for the end game. Repeat your goals frequently
* Create space for learning by failing.
* Be fair, everyone has the responsibility of taking on not to interesting parts of work.
* Lead by example
* drive for impact

### Deliver Frequently

Delivering frequently is the difference between teams that succeed in completing domain changing projects  and the teams that fail. Showing running code is more powerful than a promise to deliver something in near future.  Overtime executive support for long running project erodes but continuous deliver keeps that confidence high. Continuous delivery gives the team an opportunity to experiment with new ideas at low cost, maintain a narrow focus, and learn from & react quickly to customer feedback. 

You should
* Build the CI & CD pipeline before writing any meaningful code
* Capture customer feedback at point of use
* Plan to build a complete feature set that would be useful to a segment of users. It will help limit any jarring experience navigating between old and new stack.

### Expect Surprises 

Domain changing rewrites will deliver plenty of surprises that have the potential of derailing the effort. Surprises can come from anywhere; from the code as previously unknown capabilities, or data that reflects state resulting from buggy code in production, or long time users of the product. These surprises can come at any stage of the lifecycle. To mitigate; release early and often, put built product in front of any many stakeholders as possible and have a way to quickly correct and move forward. 

You should
* have a mindset of fixing forward, a long pause after rollback can erode confidence
* Have a pipeline to test & deliver changes quickly
* do frequent launches
* build dashboards that alert you on anomalies, and a process of review and correct them immediately
* challenge all surprise discoveries and look for ways to end support for corner cases.

### Choosing your stack

Choosing a tech stack for your application  is one of the hardest decisions you will make and will continue to question it even after the application is in production. Some factors that will influence your decision are cost, skills of the team, type of the application you are developing (commerce, learning portal or marketplace), security (will you accept payments), control over data, non functional requirements (availability, scale, response times). Just think you are building something for the next ten years. No matter which stack you choose do should,

* deploy everything to the cloud
* Containerize and automate 100% of deployment steps to easily add new environments, stay up to date on security and scale the application
* implement domain capabilities as microservices; they are easier to build and manage and worth the the added cost of orchestration and data ownership.
* build reusable UI widgets to bring consistency beyond the current application.
* depend on cloud managed capabilities (messaging, database, file store, cache) to reduce the burden of operations and tuning 

### Be creative

You will run into new challenges everyday, be creative and experiment.

Long tail of task is tedious and unpredictable, during this phase consider switching to [Kanban](https://en.wikipedia.org/wiki/Kanban).

Keep moving forward, if you depend on a team who is asking for time to deliver a perfect solution to you, do not wait. Work with what is available now and your architecture should allow you to come back to it when next revision of the dependency is ready otherwise you will be stuck in a web of promises.

Take the time to design things the right way even if it takes long time, well built components last longer and lead to reuse which saves time elsewhere.