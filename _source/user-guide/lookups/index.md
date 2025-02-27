---
layout: article
title: Lookup lists
permalink: /user-guide/lookups/
image: https://dytvr9ot2sszz.cloudfront.net/logz-docs/social-assets/docs-social.jpg
description: Learn about Logz.io's lookup lists
flags:
  logzio-plan: pro
tags:
  - security-analytics
  - lookups
  - kibana-filters
  - security-rules
contributors:
  - shalper
  - imnotashrimp
  - yberlinger
  - danielberman
---


Lookup lists are custom lists that you can use for simpler, easier query filtering in Kibana. 
Instead of adding a long list of elements to your query, you can create lookup lists and use them to filter results by adding the operator `in lookups` or `not in lookups`. For example, you can create lookup lists of allowlisted or blocklisted usernames, IP addresses, regions, or domains. 

Each list you create is added to the main Lookup lists library: Because the lookup lists are centrally managed, any list can be easily updated and changed without requiring manually updating multiple dashboards, saved searches, security rules, and so on.

To view and create lookup lists, from the **Cloud SIEM** menu, go to [**More Options > Lookups**](https://app.logz.io/#/dashboard/security/rules/lookup).

![Open Lookup lists](https://dytvr9ot2sszz.cloudfront.net/logz-docs/siem-lookups/lookuplist-nav.gif)

## Static and Dynamic lookups

Logz.io offers two main lookup lists, Static and Dynamic. 

A **Static lookup list** is created by adding individual values or uploading a CSV file with the different fields and values you want to track. While you can update the list, it has to be done manually and maintained by you.

a **Dynamic lookup list** uses a query as its source of data. The query's results fill the list with the different fields and values, updating and maintaining it independently.

Learn more about the two lookup lists and how to use them:

* **[Create a Static lookup](/user-guide/lookups/static-lookup.html)**
* **[Create a Dynamic lookup](/user-guide/lookups/dynamic-lookup.html)**
* [Filter by lookup lists in Kibana](/user-guide/lookups/#filter-by-lookup-lists-in-kibana)
* [Add a lookup list filter to a security rule](/user-guide/lookups/#add-a-lookup-list-filter-to-a-security-rule)
* [Delete a lookup list](/user-guide/lookups/#delete-a-lookup-list)

## Filter by lookup lists in Kibana

You can filter by lookup lists in Kibana dashboards, security rules, and searches.

For example, go to the SIEM [Kibana](https://app.logz.io/#/dashboard/security/research) page or open a Dashboard. Click **Add a filter** to show the filter dialog box.

![Filter by lookup](https://dytvr9ot2sszz.cloudfront.net/logz-docs/siem-lookups/lookup_filter-kibana_or_dashbd.gif)

* **Field** - Select a field to filter by.
* **Operator** - Select the operator **in lookups** or **not in lookups**.
* **Value** - Select the lookup you want to filter by.

## Add a lookup list filter to a security rule

If you want to use your lookup lists as a reference when creating security rules, navigate to the [Create a rule](https://app.logz.io/#/dashboard/security/rules/v2019/new) page, and select **Add a filter**.

Select the field you want to filter by, and select whether it's included or excluded from a lookup.

Next, select the lookup list you'd like to refer to from the dropdown menu.

![Select a filter](https://dytvr9ot2sszz.cloudfront.net/logz-docs/siem-lookups/filter-with-lookup_rules.png)

**Save** your filter, and continue editing the rule.

[Learn more about managing security rules]({{site.baseurl}}/user-guide/cloud-siem/manage-security-rules.html).

## Delete a lookup list

To delete a lookup, hover over the item and click **delete** <i class="li li-trash"></i> to delete it. You'll be asked to confirm the deletion.

