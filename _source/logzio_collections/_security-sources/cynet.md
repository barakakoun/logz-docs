---
title: Ship logs from Cynet
logo:
  logofile: cynet.png
  orientation: vertical
data-source: Cynet
data-for-product-source: Cloud SIEM
templates: ["network-device-filebeat"]
contributors:
  - nshishkin
shipping-tags:
  - firewalls
order: 1380
---
[Cynet](https://www.cynet.com/platform/) is a cybersecurity asset management platform. This topic describes how to send system logs from your Cynet platform to Logz.io.

**Before you begin, you'll need**: 

* An active Cynet license
* Cynet login credentials 
* An active account with Logz.io
* [Filebeat](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation.html) installed on a dedicated machine (acting as a syslog server)
* Root priveleges on your machines 

<div class="tasklist">


##### Configure Cynet to send syslog notifications to a remote Syslog server running Filebeat

1. On your Cynet web interface, go to **Setting > Advanced**.
![Advanced options](https://dytvr9ot2sszz.cloudfront.net/logz-docs/Cynet/cynet1.png)
2. Select the box beside **Send Audit Records to SIEM**.
3. Go to **Configuration > SIEM settings** and enable the following configuration:
![SIEM settings before](https://dytvr9ot2sszz.cloudfront.net/logz-docs/Cynet/cynet2before.jpg)
   * **UDP**
   * **IP** - public IP address of your syslog server
   * **Port** - port that is configured on your syslog server. We use 9000 in this example, but you can change it to your preference.
4. Press **Add**. The added IP and port will appear on the screen.
![SIEM settings after](https://dytvr9ot2sszz.cloudfront.net/logz-docs/Cynet/cynet2after.jpg)
  
<!-- info-box-start:info -->
These instructions are based on UDP. If you want to use TCP, make sure your syslog server configuration is aligned with this.
{:.info-box.note}
<!-- info-box-end -->

{% include log-shipping/certificate.md %}


##### Configure Filebeat

1. Paste the following into the inputs section of the Filebeat configuration file:

   ```yaml
   filebeat.inputs:
   - type: udp
     max_message_size: 10MiB
     host: "0.0.0.0:9000"
     fields:
       logzio_codec: json
       # Your Logz.io account token. You can find your token at
       #  https://app.logz.io/#/dashboard/settings/manage-accounts
       token: <<LOG-SHIPPING-TOKEN>>
       type: cynet
     fields_under_root: true
     encoding: utf-8
     ignore_older: 3h
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
   output.logstash:
     hosts: ["<<LISTENER-HOST>>:5015"]
     ssl:
       certificate_authorities: ['/etc/pki/tls/certs/COMODORSADomainValidationSecureServerCA.crt']
   ```
 
   * 9000 is the port we suggest. If you use a different port, replace the default values with your parameters.
   * {% include log-shipping/log-shipping-token.md %}
   * {% include log-shipping/listener-var.md %}

2. Run Filebeat with the new configuration.

##### Check Logz.io for your logs

Give your logs some time to get from your system to ours, and then open [Kibana](https://app.logz.io/#/dashboard/kibana/discover?). You can filter for data of type `cynet` to see the incoming logs.
  
If you still don’t see your data, see [log shipping troubleshooting](https://docs.logz.io/user-guide/log-shipping/log-shipping-troubleshooting.html).

</div>
