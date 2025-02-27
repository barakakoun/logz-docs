---
layout: article
title: Correlate logs and traces
permalink: /user-guide/distributed-tracing/correlate-traces
image: https://dytvr9ot2sszz.cloudfront.net/logz-docs/social-assets/docs-social.jpg
description: Correlate Kibana and Jaeger for more context and insights
flags:
  logzio-plan: pro enterprise
tags:
  - distributed tracing
contributors:
  - yberlinger
---

## What can I do with my trace and span data in Kibana?

Make the Jaeger-Kibana connection: Harness the power of Kibana and Jaeger for more context and insights with Logz.io Distributed Tracing when you correlate traces with logs in Kibana.

We’ve put together examples of visualizations over distributed tracing telemetry that we think you’ll find helpful. 

## Telemetry correlation

If you’re using Logz.io for both Log Analytics and Distributed Tracing, you can correlate logs with their corresponding traces and spans.

To search for the logs related to a specific trace or the request transaction which generated the trace, you’ll need to enrich your logs with the trace ID and span ID. These log fields will let you drill down via a link that opens the specific trace in Jaeger from within Kibana Discover, or within a Kibana Dashboard.

Enriching logs with trace ID and span ID must be performed before or during the data collection process, and before sending the data to Logz.io.

The enrichment process depends on the language and log type: You will have to [configure your deployment](/user-guide/distributed-tracing/deploying-components) to perform these actions _before_ the data is ingested into Logz.io, because retroactive correlation is not available.  

Contact <a class="intercom-launch" href="mailto:help@logz.io">the Support team for additional information on enriching your logs. </a>.

### Correlating Logs and Traces

1. Once the trace ID is part of the log attributes, open the Kibana left menu, and select **Management**.
2. To manage log index patterns, click **Index pattern** and go to your default index pattern settings.
    ![Go to index pattern settings](https://dytvr9ot2sszz.cloudfront.net/logz-docs/distributed-tracing/logs-management-index.png)
3. Search for the trace ID field you want to correlate with your logs and select **Edit**. In this example, the field name is **traceID**.
    ![search for and edit the traceID](https://dytvr9ot2sszz.cloudfront.net/logz-docs/distributed-tracing/logs-traceid-edit.png)
4. Change the **Format** to **URL** and enable **Open in a new tab**.
    ![Change format and open](https://dytvr9ot2sszz.cloudfront.net/logz-docs/distributed-tracing/logs-edit-traceid.png)
5. Using your main account, insert the following template in the **URL template** field of the Jaeger user interface (UI), and **Save field**.
  
    `https://app.logz.io/#/dashboard/jaeger/trace/{{value}}`

Each traceID attribute functions as a drill down link that leads you to the correlated trace view in Jaeger. 
![traceID to Trace view](https://dytvr9ot2sszz.cloudfront.net/logz-docs/distributed-tracing/log_trace5-new.png)

The Trace ID identifies logs related to traces, and it should be a part of the log. If you're using OpenTelemetry as instrumentation, you can get the Trace ID in context from the OTEL library and attach it to the log on creation. 
{:.info-box.note}

### From traceID to Kibana
You can jump from a trace in Jaeger to view it in Kibana: 
![traceID lookup](https://dytvr9ot2sszz.cloudfront.net/logz-docs/distributed-tracing/view-in-kibana-new.png)