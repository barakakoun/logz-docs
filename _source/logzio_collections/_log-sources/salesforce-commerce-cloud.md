---
title: Ship Salesforce Commerce Cloud data to Logz.io
logo:
  logofile: salesforce-commerce-cloud-logo.png
  orientation: vertical
data-source: Salesforce Commerce Cloud
data-for-product-source: Logs
open-source:
  - title: Salesforce Commerce Cloud data fetcher
    github-repo: sfcc-webdav-fetcher
contributors:
  - nshishkin
shipping-tags:
  - popular
order: 470
---

<!-- tabContainer:start -->
<div class="branching-container">

* [Overview](#overview)
* [Default configuration](#default)
* [Configuration by config file](#config)
{:.branching-tabs}

<!-- tab:start -->
<div id="overview">

Salesforce Commerce Cloud is a scalable, cloud-based software-as-a-service (SaaS) ecommerce platform. This integration allows you to collect data from Salesforce Commerce Cloud and send it to your Logz.io account.

</div>
<!-- tab:end -->

<!-- tab:start -->
<div id="default">

The default configuration uses a Docker container with environment variables.

<div class="tasklist">


##### Pull the Docker image of the Logz.io Salesforce Commerce Cloud data fetcher

```shell
docker pull logzio/sfcc-logs-fetcher:latest
```

##### Run the Docker container

```shell
docker run -d -e LOGZIO_SHIPPING_TOKEN=<logzio_shipping_token>
-e LOGZIO_LISTENER_URL=<logzio_listener_url> \
-e SFCC_SERVER_NAME=<your_sfcc_host> \
-e SFCC_CLIENT_ID=<your_sfcc_client_id> \
-e SFCC_LOG_SOURCE=operational
-e SFCC_CLIENT_SECRET=<your_sfcc_client_secret> \
logzio/sfcc-logs-fetcher:latest
```

| Parameter | Description | Required/Default |
|---|---|---|
|  `<<LOG-SHIPPING-TOKEN>>` | Your Logz.io account token. {% include log-shipping/log-shipping-token.html %}  | Required  |
| `<<LOGZIO_LISTENER_URL>>` | {% include log-shipping/listener-var.md %} | Required |
| SFCC_SERVER_NAME         | SFFC Server name from where you would like to collect logs.                                                                                                                                                                                                                                                                 |      Required |
| SFCC_CLIENT_ID        | Client id related to the account that you need to send logs from. [Learn more](https://documentation.b2c.commercecloud.salesforce.com/DOC3/index.jsp?topic=%2Fcom.demandware.dochelp%2Fcontent%2Fb2c_commerce%2Ftopics%2Faccount_manager%2Fb2c_account_manager_add_api_client_id.html)                                                           |      Required |
| SFCC_LOG_SOURCE    | Flag to represent which log types you would like to collect. `operational` for operational logs, `security` for security logs, `all` for both of them.                                                       |      Required |
| SFCC_CLIENT_SECRET    | Client secret related to the account that you need to send logs from. [Learn more](https://documentation.b2c.commercecloud.salesforce.com/DOC3/index.jsp?topic=%2Fcom.demandware.dochelp%2Fcontent%2Fb2c_commerce%2Ftopics%2Faccount_manager%2Fb2c_account_manager_add_api_client_id.html)                                                       |      Required |

##### Check Logz.io for your logs

Give your logs some time to get from your system to ours, and then open [Kibana](https://app.logz.io/#/dashboard/kibana).

If you still don't see your logs, see [log shipping troubleshooting]({{site.baseurl}}/user-guide/log-shipping/log-shipping-troubleshooting.html).

</div>

</div>
<!-- tab:end -->


<!-- tab:start -->
<div id="config">

The default configuration uses a Docker container with environment variables defined by a configuration file.

<div class="tasklist">


##### Create a local directory for this integration

You will need a dedicated directory to use it as mounted directory for the Docker container of the Logz.io Salesforce Commerce Cloud.

```shell
mkdir sfcc-logs-fetcher
cd sfcc-logs-fetcher
```

##### Create a configuration file

In the directory created in the previous step, create a file `variables.env` as follows:

```env
# Environment file for Logz.io Logs for SalesForce Commerce Cloud
LOGZIO_SHIPPING_TOKEN=<<LOG-SHIPPING-TOKEN>>
SFCC_SERVER_NAME=<<your_sfcc_host>>
SFCC_CLIENT_ID=<<your_sfcc_client_id>>
SFCC_CLIENT_SECRET=<<your_sfcc_client_secret>>
CCTAIL_ARGS=sfcc
LOGZIO_LISTENER_URL=https://listener.logz.io:8071
```

Replace the variable values as per the table below:

| Parameter | Description | Required/Default |
|---|---|---|
| LOGZIO_SHIPPING_TOKEN | `<<LOG-SHIPPING-TOKEN>>` Your Logz.io account token. {% include log-shipping/log-shipping-token.html %}  | Required  |
| LOGZIO_LISTENER_URL | {% include log-shipping/listener-var.md %} | Required |
| SFCC_SERVER_NAME         | SFFC Server name from where you would like to collect logs (ex. `dev01-mysandbox.demandware.net`)                                                                                                                                                                                                                                                                |      Required |
| SFCC_CLIENT_ID        | Client id related to the account that you need to send logs from. [Learn more](https://documentation.b2c.commercecloud.salesforce.com/DOC3/index.jsp?topic=%2Fcom.demandware.dochelp%2Fcontent%2Fb2c_commerce%2Ftopics%2Faccount_manager%2Fb2c_account_manager_add_api_client_id.html)                                                           |      Required |
| SFCC_CLIENT_SECRET    | Client secret related to the account that you need to send logs from. [Learn more](https://documentation.b2c.commercecloud.salesforce.com/DOC3/index.jsp?topic=%2Fcom.demandware.dochelp%2Fcontent%2Fb2c_commerce%2Ftopics%2Faccount_manager%2Fb2c_account_manager_add_api_client_id.html)                                                       |      Required |

##### Run the Docker container with the configuration file 2

```shell
docker run -d --env-file=variables.env logzio/webdav-fetcher:latest
```


##### Check Logz.io for your logs

Give your logs some time to get from your system to ours, and then open [Kibana](https://app.logz.io/#/dashboard/kibana). You can filter for data of type `cloud_commerce` to see the incoming Salesforce Commerce Cloud logs. 

If you still don't see your logs, see [log shipping troubleshooting]({{site.baseurl}}/user-guide/log-shipping/log-shipping-troubleshooting.html).

</div>

</div>
<!-- tab:end -->
</div>
<!-- tabContainer:end -->
