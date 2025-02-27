---
title: Ship Fail2ban logs
logo:
  logofile: fail2ban.png
  orientation: vertical
data-source: Fail2ban
data-for-product-source: Cloud SIEM
templates: [beats-logs]
contributors:
  - imnotashrimp
shipping-tags:
  - linux
order: 560
---
Fail2Ban is an intrusion prevention software framework that protects computer servers from brute-force attacks. This integration allows you to send Fail2ban logs to your Logz.io SIEM account.

#### Guided configuration

**Before you begin, you'll need**:
[Filebeat](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation.html)
[Fail2ban](https://www.fail2ban.org/wiki/index.php/Downloads),
root access

<div class="tasklist">

{% include log-shipping/certificate.md %}

##### Add Fail2ban as an input

In the Filebeat configuration file (/etc/filebeat/filebeat.yml), add Fail2ban to the filebeat.inputs section.

{% include log-shipping/log-shipping-token.html %}

```yaml
# ...
filebeat.inputs:
- type: log

  paths:
  - /var/log/fail2ban.log

  fields:
    logzio_codec: plain

    # Your Logz.io account token. You can find your token at
    #  https://app.logz.io/#/dashboard/settings/manage-accounts
    token: <<LOG-SHIPPING-TOKEN>>
    type: fail2ban
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

Give your logs some time to get from your system to ours, and then open [Kibana](https://app.logz.io/#/dashboard/kibana).

If you still don't see your logs, see [log shipping troubleshooting]({{site.baseurl}}/user-guide/log-shipping/log-shipping-troubleshooting.html).

</div>
