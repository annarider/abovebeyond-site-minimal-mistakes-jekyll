---
title: "Is this one setting significantly slowing down your xDM integration jobs?"
categories:
  - Integration
  - Semarchy xDM
tags:
  - performance
  - certification
  - integration job
---

If you're looking for a quick win, I suggest that you turn off Analyze Stats, which you can do in 1 minute. (Learn more about PARAM_ANALYZE_STATS.)

I noticed in the integration job logs that your Historization and Consolidate steps spend a significant amount of time on analyzing statistics. I was surprised to see that this setting is still turned on for integration jobs. Here's my analysis (sorted Time by descending): 
image.png

Here's some background information on PARAM_ANALYZE_STATS in case you're unfamiliar with it: 

The default behavior of PARAM_ANALYZE_STATS is set to true (1), meaning statistics are gathered for jobs by default. 

If PARAM_ANALYZE_STATS is set to 1, statistics collection is triggered in the xDM data location tables to optimize processing. This option is useful to accelerate the processing of large data sets. The downside is that turning on stats slows down your integration jobs.

When should PARAM_ANALYZE_STATS be set to True (value = 1)?

This parameter should be set to true whenever you create / modify more than 5% of the records in the data location or for first-time loads. Typically for an initial load it's much better to have the statistics gathering enabled. After the initial load, gathering statistics for each and every job results in slowing down integration jobs.

Another reason to set the parameter to 1 is when you are trying to load a large number of records (in the millions) as the statistics will be used to optimize performance.

As a rule of thumb, data entry jobs and any job that processes less than 20,000 records should never collect statistics

Disable Analyze Stats in most cases because production databases have a maintenance plan and daily statistics gathering tasks can be scheduled nightly for the whole schema.

How to override the default value

Go to Model Design
Go into Jobs
Double click on the job that you want to change the default behaviour
Click on Job Parameters
Add a new Parameter: 
Name: PARAM_ANALYZE_STATS
Value: 0
image.png
Feel free to look at the execution logs at the individual steps like I mentioned in my previous email first before changing the PARAM_ANALYZE_STATS job parameter to confirm that stats gathering is adding significant time to the processing before you turn off Analyze Stats.
