---
layout: post
title: "Databases in Business"
date: 2015-10-15 19:52:09 -0400
comments: true
categories: Flatiron School
---
Learning a programming language often involves thinking in abstractions about things that don't really exist. Particularly at the absolute beginner level, it can be challenging to see the big picture behind organizing pigeons and pokemon, and the level of frustration these exercises are capable of provoking seems highly disproportionate to the problem we're trying to solve. My rational mind understands that we're working toward something much bigger than pigeons and pokemon, but it can be easy to lose sight of the end goal. 

{% img left http://placekitten.com/320/250 Catabase %}
One thing that helps me reconcile this frustration is to understand how these abstract concepts are applicable in the real world. When I can see the link between what we're learning and it's potential to effect real change in real things used by real people, I get excited. And databases *really* excite me. 

This has not always been the case. In business school, the concept of database functionality in Enterprise Resource Planning systems (ERPs) was a similar blur of abstraction. I later began to appreciate their importance in ERPs while working as a financial auditor, and this appreciation has only grown since starting my own business (which happens to be centered on data integrity). Now, as we're learning how to manipulate relational data with Ruby, I decided to look back at those ERPs concepts and see how they're relevant to a budding programmer.

{% img right http://www.inddist.com/sites/inddist.com/files/ERP-Graphic.jpg 450 450 %}
So, what's an ERPs, and why should we care?

Basically, an ERPs is an interactive database of databases. More formally, it’s a suite of integrated software applications that transmit sets of data to one another as they relate to a business’s resources - ex: cash balances, payment obligations, inventory usage, production capacity, payroll, sales revenue, etc. Each department in the business will typically collect, store, manage and/or interpret the data generated from within the department, and the ERP facilitates the flow of data between the departments. For example, the Finance/Accounting department would be able to trace an increase in expenses to the Manufacturing department, whose records show increased production of product X, the increased production of which was prompted by increased demand for product X per the data obtained from Customer Relations.

Demo: http://www.weberp.org/weberp/index.php

Where older systems limited departmental data to local networks, the web-based software in ERPs permits employees, customers and suppliers real-time access to data. This is relevant to developers as the push to even greater levels of interactivity has prompted increased efforts to integrate mobile devices with current systems.

{% img left http://www.vkinfotek.com/hpi/erpappdesign.gif %}

Other considerations...:

- Lots of opportunity in ERP software development due to widespread implementation, evolving systems, and customization of systems
- Requires that the programming team has a solid understanding of the organization's specific business processes (maybe even better than the employees have themselves)
- Scope of design
- MVC!!!

