---
title: Ship Puppet logs
logo:
  logofile: puppet.png
  orientation: vertical
data-source: Puppet
data-for-product-source: Logs
templates: [beats-logs]
contributors:
  - amosd92
  - imnotashrimp
  - shalper
shipping-tags:
  - ci-cd
order: 1020
---
Puppet is a software configuration management tool which includes its own declarative language to describe system configuration. Deploy this integration to send logs from your Puppet applications to your Logz,io account. 

#### Configuration

**Before you begin, you'll need**:

* [Filebeat](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation.html) installed
* Root access
* Port 5015 open

<div class="tasklist">

{% include log-shipping/certificate.md %}

##### Add Puppet as an input

In the Filebeat configuration file (/etc/filebeat/filebeat.yml), add Puppet to the filebeat.inputs section.


<!-- info-box-start:info -->
We recommend configuring Puppet to output JSON logs. See [Advanced Logging Configuration](https://puppet.com/docs/puppetserver/5.1/config_logging_advanced.html) from Puppet for more information.
{:.info-box.tip}
<!-- info-box-end -->

{% include log-shipping/log-shipping-token.html %}

```yaml
# ...
filebeat.inputs:
- type: log
  paths:

  # If you configured Puppet to output JSON logs, replace the filename with
  #  `puppetserver.log.json`
  - /var/log/puppetlabs/puppetserver/puppetserver.log

  fields:

    # If you configured Puppet to output JSON logs, set logzio_codec to
    #  `json`
    logzio_codec: plain

    # You can manage your tokens at
    # https://app.logz.io/#/dashboard/settings/manage-tokens/log-shipping
    token: <<LOG-SHIPPING-TOKEN>>
    type: puppetserver
  fields_under_root: true
  encoding: utf-8
  ignore_older: 3h

- type: log
  paths:

  # If you configured Puppet to output JSON logs, replace the filename with
  #  `puppetserver-access.log.json`
  - /var/log/puppetlabs/puppetserver/puppetserver-access.log

  fields:

    # If you configured Puppet to output JSON logs, set logzio_codec to
    #  `json`
    logzio_codec: plain

    # You can manage your tokens at
    # https://app.logz.io/#/dashboard/settings/manage-tokens/log-shipping
    token: <<LOG-SHIPPING-TOKEN>>
    type: puppetserver-access
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

The above assumes the following defaults for Puppet server logs:

* Log location (plain text format)- `/var/log/puppetlabs/puppetserver/puppetserver.log`
* Log location (JSON format)- `/var/log/puppetlabs/puppetserver/puppetserver.log.json`
* Default log type - `puppetserver`

Defaults for Puppet server access logs:

* Log location (plain text format) - `/var/log/puppetlabs/puppetserver/puppetserver-access.log`
* Log location (JSON format) - `/var/log/puppetlabs/puppetserver/puppetserver-access.log.json`
* Default log type - `puppetserver-access`

Puppet produces lots of different logs. [Learn more from Puppet Labs Docs 🔗](https://puppet.com/docs/pe/2018.1/what_gets_installed_and_where.html#log-files-installed).

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

Give your logs some time to get from your system to ours, and then open [Kibana](https://app.logz.io/#/dashboard/kibana). You can search for `type:puppetserver-access OR puppetserver` to filter for your logs. Your logs should be already parsed thanks to the Logz.io preconfigured parsing pipeline.


If you still don't see your logs, see [log shipping troubleshooting]({{site.baseurl}}/user-guide/log-shipping/log-shipping-troubleshooting.html).

</div>
