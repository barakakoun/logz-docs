---
title: Ship nginx logs
logo:
  logofile: nginx.svg
  orientation: horizontal
data-source: nginx
data-for-product-source: Cloud SIEM
templates: [beats-logs]
contributors:
  - amosd92
  - imnotashrimp
  - shalper
shipping-tags:
  - server-app
  - popular
order: 340
---
Nginx is a web server that can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache. Deploy this integration to ship Nginx metrics, including Plus API, Plus, Stream STS, VTS. This integration allows you to send nginx logs to your Logz.io SIEM account.

#### Filebeat configuration

**Before you begin, you'll need**:

* [Filebeat](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation.html)
* Root access
* Port 5015 open

<div class="tasklist">

{% include log-shipping/certificate.md %}

##### Add nginx as an input

In the Filebeat configuration file (/etc/filebeat/filebeat.yml), add nginx to the filebeat.inputs section.

{% include log-shipping/log-shipping-token.html %}

```yaml
# ...
filebeat.inputs:
- type: log
  paths:
  - /var/log/nginx/access.log

  fields:
    logzio_codec: plain

    # Your Logz.io account token. You can find your token at
    #  https://app.logz.io/#/dashboard/settings/manage-accounts
    token: <<LOG-SHIPPING-TOKEN>>
    type: nginx_access
  fields_under_root: true
  encoding: utf-8
  ignore_older: 3h

- type: log
  paths:
  - /var/log/nginx/error.log

  fields:
    logzio_codec: plain

    # Your Logz.io account token. You can find your token at
    #  https://app.logz.io/#/dashboard/settings/manage-accounts
    token: <<LOG-SHIPPING-TOKEN>>
    type: nginx_error
  fields_under_root: true
  encoding: utf-8
  ignore_older: 3h
```

If you're running Filebeat 8.1+, the `type` of the `filebeat.inputs` is `filestream` instead of `logs`:

```yaml
filebeat.inputs:
- type: filestream
  paths:
    - /var/log/*.log
```

If you're running Filebeat 7, paste this code block.
Otherwise, you can leave it out.

```yaml
# ... For Filebeat 7 only ...
filebeat.registry.path: /var/lib/filebeat
processors:
- rename:
    fields:
    - from: "agent"
      to: "filebeat_agent"
    ignore_missing: true
- rename:
    fields:
    - from: "log.file.path"
      to: "source"
    ignore_missing: true
```

If you're running Filebeat 6, paste this code block.

```yaml
# ... For Filebeat 6 only ...
registry_file: /var/lib/filebeat/registry
```

The above assumes the following defaults for Access logs:

* Log location - `/var/log/nginx/access.log`
* Log type - `nginx`, `nginx_access`, or `nginx-access`

Defaults for Error logs:

* Log location - `/var/log/nginx/error.log`
* Log type - `nginx-error`

##### Set Logz.io as the output

If Logz.io is not an output, add it now.
Remove all other outputs.

{% include log-shipping/listener-var.html %} 

```yaml
# ...
output.logstash:
  hosts: ["<<LISTENER-HOST>>:5015"]
  ssl:
    certificate_authorities: ['/etc/pki/tls/certs/COMODORSADomainValidationSecureServerCA.crt']
```

##### Start Filebeat

[Start or restart Filebeat](https://www.elastic.co/guide/en/beats/filebeat/master/filebeat-starting.html) for the changes to take effect.

##### Check Logz.io for your logs

Confirm you're shipping logs by opening an nginx-hosted webpage in your browser. Give your logs some time to get from your system to ours, and then open [Kibana](https://app.logz.io/#/dashboard/kibana).

You can search for `type:nginx OR nginx_access OR nginx-access OR nginx-error` to filter for your logs. Your logs should be already parsed thanks to the Logz.io preconfigured parsing pipeline.



If you still don't see your logs, see [log shipping troubleshooting]({{site.baseurl}}/user-guide/log-shipping/log-shipping-troubleshooting.html).

</div>
