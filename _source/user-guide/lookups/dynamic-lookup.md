---
layout: article
title: Dynamic lookups
permalink: /user-guide/lookups/dynamic-lookup.html
image: https://dytvr9ot2sszz.cloudfront.net/logz-docs/social-assets/docs-social.jpg
description: Learn about Logz.io's Dynamic lookups and how you can utilize them
flags:
  logzio-plan: pro
tags:
  - security-analytics
  - lookups
  - kibana-filters
  - security-rules
contributors:
  - hidan
---


Lookup lists let you upload a list that includes elements and use it as a reference to look for matches across your code. For example, you can create lookup lists of allowlisted or blocklisted usernames, IP addresses, regions, or domains.

**Dynamic lookups** are an extension of lookup lists, but unlike the latter, they actively and continuously search for matches across your logs. 

You can create Dynamic lookups based on your Kibana query and initiate a rule for your list. The list will automatically retrieve the data on a timely basis and populate a list of values based on a specific field from the query result, which you can [use when creating a security rule](/user-guide/lookups/#add-a-lookup-list-filter-to-a-security-rule). 

#### How to create a Dynamic lookup

Navigate to [Lookups](https://app.logz.io/#/dashboard/security/lookups) > [New Dynamic lookup](https://app.logz.io/#/dashboard/security/lookups/dynamic/new).

First, name your new lookup and add a short description. This will help you and your teammates identify what it contains. 

Enter the query you want for the list. To narrow down your results, you can add one or more filters. To test whether the query has any results, click on Preview in Kibana.

![Enter lookup query](https://dytvr9ot2sszz.cloudfront.net/logz-docs/siem-lookups/dynamic-lookup-query.png)

Next, select the field for the lookup. The dropdown list includes all of the fields available in your account, and you can type its name to search and find it quickly.

![Select lookup field](https://dytvr9ot2sszz.cloudfront.net/logz-docs/siem-lookups/lookup-field-dropdown.png)

Choose the time frame intervals in which you want the query to run. It can be anywhere between 1 hour and up to 7 days. 

You can also choose how often to update the values inside the lookup list using a [Cron expression](https://www.freeformatter.com/cron-expression-generator-quartz.html#cronexpressionexamples/).

![Choose time intervals](https://dytvr9ot2sszz.cloudfront.net/logz-docs/siem-lookups/lookup-time-interval.png)

When you’re done, click on **Save**. You’ll be redirected to the main lookup page, where you’ll be able to view all of the lookup lists generated in your account, both Static and Dynamic.


#### View Dynamic lookup dynamic elements


Once the lookup list is up and running, you can go back to it to view the dynamic elements it found. Navigate to [Lookups](https://app.logz.io/#/dashboard/security/lookups), find the lookup list and click the edit button.

![Choose time intervals](https://dytvr9ot2sszz.cloudfront.net/logz-docs/siem-lookups/dynamic-elements-region.png)

##### Additional resources

Logz.io offers 2 additional methods to create lookup lists: 

*  Create a [Static lookup lists](/user-guide/lookups/static-lookup.html)
* Upload your lookup from a [CSV file](/user-guide/lookups/static-lookup.html#create-or-edit-a-lookup-list-via-csv-file-upload)
