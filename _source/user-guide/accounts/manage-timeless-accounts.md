---
layout: article
title: Manage timeless accounts
permalink: /user-guide/accounts/manage-timeless-accounts.html
flags:
  admin: true
  logzio-plan: pro
tags:
  - accounts
  - timeless-accounts
contributors:
  - imnotashrimp
  - ayigal
  - hidan
---

While your main account and sub accounts
are better suited for short-term monitoring of your log data,
they're not really a cost-effective way to view long-term patterns and trends.
That's where timeless accounts come in.

Timeless accounts work with optimizers,
allowing you to save a subset of your logs for as long as you need them.
You can store key metrics that you want to track in your timeless accounts.
Each timeless account contains its own Elasticsearch index,
which you can search from your main account.

![Timeless accounts](https://dytvr9ot2sszz.cloudfront.net/logz-docs/accounts/accounts--timeless-accounts.png)

#### To create or manage a timeless account

<div class="tasklist">

##### Select or add a timeless account

In the **Timeless account plan** panel,
click the timeless account you want to manage,
or click **Add timeless account**.

![Manage a timeless account](https://dytvr9ot2sszz.cloudfront.net/logz-docs/accounts/accounts--manage-timeless-account.png)

##### Adjust account volume

If you need to adjust the **Timeless account volume**,
type in the text box
or use <i class="li li-plus"></i> and <i class="li li-minus"></i>.

Your plan allows for a maximum data volume.
Keep this limit in mind when you allocate resources to your timeless accounts—you won't be able to exceed the limits of your plan. \\
\\
Contact your Logz.io account manager if you need to increase your plan limits.
{:.info-box.note}

##### Set search permissions

If you want your sub accounts to be able to search this account,
add the intended accounts to the **Read & write permissions** box.

##### Save your changes

Click **Apply** to save your changes.

</div>

#### To clean a timeless account


<div class="tasklist">

##### Find your timeless account plan

Log into your Logz.io account, navigate to **[Manage accounts](https://app.logz.io/#/dashboard/settings/manage-accounts)**, and find your timeless account plan.

![Timeless account plan](https://dytvr9ot2sszz.cloudfront.net/logz-docs/accounts/timeless-account-manage.png)

##### Choose the relevant account

Click on the account you'd like to clean. Then, in the dropdown menu, you'll be able to review the account's data usage.

![Timeless account plan expanded](https://dytvr9ot2sszz.cloudfront.net/logz-docs/accounts/timeless-account-in-use.png)

##### Clean the data

Click on the **Clean** button to open a popup with additional options. You can decide what log data to delete from your timeless account in this popup. Select the date range in which you'd like to clean the data. You can also select specific optimizers to clean. Next, click on **Continue**.

![Clean timeless data](https://dytvr9ot2sszz.cloudfront.net/logz-docs/accounts/timeless-date-and-optimizers-msg.png)

##### Confirm the clean

The final confirmation window summarizes how many log messages will be deleted in this process. This process might take a few minutes, depending on the number of log messages, and is irreversible. To confirm, click on the **Yes, please clean my account** button.

![Timeless account plan expanded](https://dytvr9ot2sszz.cloudfront.net/logz-docs/accounts/timeless-confirmation-msg.png)


</div>