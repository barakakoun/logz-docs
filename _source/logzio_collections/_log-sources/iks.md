---
title: Ship IKS logs
logo:
  logofile: iks.png
  orientation: vertical
data-source: IBM Cloud Kubernetes Service
data-for-product-source: Logs
templates: ["k8s-daemonset"]
open-source:
  - title: logzio-k8s
    github-repo: logzio-k8s
contributors:
  - mirii1994
  - shalper
shipping-tags:
  - container
order: 1010
---

<!-- tabContainer:start -->
<div class="branching-container">

* [Overview](#overview)
* [Default configuration](#default-config)
* [Custom configuration](#custom-config)
* [Multiline logs](#multiline)
* [Troubleshooting](#troubleshooting)
{:.branching-tabs}


<!-- tab:start -->
<div id="overview">

Fluentd is an open source data collector and a great option because of its flexibility. This implementation uses a Fluentd DaemonSet to collect Kubernetes logs and send them to Logz.io. The Kubernetes DaemonSet ensures that some or all nodes run a copy of a pod.


The image used in this integration comes pre-configured for Fluentd to gather all logs from the Kubernetes node environment and append the proper metadata to the logs. If you prefer to customize your Fluentd configuration, you can edit it before it's deployed.

<!-- info-box-start:info -->
The latest version pulls the image from `logzio/logzio-fluentd`. Previous versions pulled the image from `logzio/logzio-k8s`.
{:.info-box.note}
<!-- info-box-end -->

<!-- info-box-start:info -->
Fluentd will fetch all existing logs, as it is not able to ignore older logs.
{:.info-box.note}
<!-- info-box-end -->

###### Sending logs from nodes with taints

If you want to ship logs from any of the nodes that have a taint, make sure that the taint key values are listed in your in your daemonset/deployment configuration as follows:
  
```yaml
tolerations:
- key: 
  operator: 
  value: 
  effect: 
```
  
To determine if a node uses taints as well as to display the taint keys, run:
  
```
kubectl get nodes -o json | jq ".items[]|{name:.metadata.name, taints:.spec.taints}"
```


###### K8S version compatibility

* **K8S 1.16 or earlier** - If you're running K8S 1.16 or earlier, you may need to manually change the API version in your DaemonSet to `apiVersion: rbac.authorization.k8s.io/v1beta1`.

  The API versions of `ClusterRole` and `ClusterRoleBinding` are found in `logzio-daemonset-rbac.yaml` and `logzio-daemonset-containerd.yaml`.
  
  If you are running K8S 1.17 or later, the DaemonSet is set to use `apiVersion: rbac.authorization.k8s.io/v1` by default. No change is needed.

{% include /log-shipping/multiline-logs-fluentd.md %}


</div>

<!-- tab:end -->



<!-- tab:start -->
<div id="default-config">

For most environments, deploying logzio-k8s with the default configuration is recommended.
If your environment requires a custom configuration, follow the steps for deploying a custom configuration.


#### To deploy logzio-k8s

<div class="tasklist">

##### Create a monitoring namespace

Your DaemonSet will be deployed under the namespace `monitoring`.


```shell
kubectl create namespace monitoring
```



##### Store your Logz.io credentials

Save your Logz.io shipping credentials as a Kubernetes secret.


```shell
kubectl create secret generic logzio-logs-secret \
--from-literal=logzio-log-shipping-token='<<LOG-SHIPPING-TOKEN>>' \
--from-literal=logzio-log-listener='https://<<LISTENER-HOST>>:8071' \
-n monitoring
```

{% include /general-shipping/replace-placeholders.html %}


##### Deploy the DaemonSet

Run:

```shell
kubectl apply -f https://raw.githubusercontent.com/logzio/logzio-k8s/master/logzio-daemonset-containerd.yaml -f https://raw.githubusercontent.com/logzio/logzio-k8s/master/configmap.yaml
```

#####  Check Logz.io for your logs

Give your logs some time to get from your system to ours,
and then open [Kibana](https://app.logz.io/#/dashboard/kibana).

If you still don't see your logs,
see [log shipping troubleshooting](https://docs.logz.io/user-guide/log-shipping/log-shipping-troubleshooting.html).




</div>
</div>
<!-- tab:end -->


<!-- tab:start -->
<div id="custom-config">

You can customize the configuration of your Fluentd container by editing either your DaemonSet or your Configmap.


#### To deploy logzio-k8s

<div class="tasklist">


##### Create a monitoring namespace

Your DaemonSet will be deployed under the namespace `monitoring`.


```shell
kubectl create namespace monitoring
```


#####  Store your Logz.io credentials

Save your Logz.io shipping credentials as a Kubernetes secret.


```shell
kubectl create secret generic logzio-logs-secret \
--from-literal=logzio-log-shipping-token='<<LOG-SHIPPING-TOKEN>>' \
--from-literal=logzio-log-listener='https://<<LISTENER-HOST>>:8071' \
-n monitoring
```

{% include /general-shipping/replace-placeholders.html %}

##### Configure Fluentd

Download Logz.io's [Containerd DaemonSet](https://raw.githubusercontent.com/logzio/logzio-k8s/master/logzio-daemonset-containerd.yaml) and open it in your text editor to edit it.

If you wish to make advanced changes in your Fluentd configuration, you can download and edit the [configmap yaml file](https://raw.githubusercontent.com/logzio/logzio-k8s/master/configmap.yaml).


{% include k8s-fluentd.md %}


##### Deploy the DaemonSet

Run:

```shell
kubectl apply -f path/logzio-daemonset-containerd.yaml -f path/configmap.yaml
```

Replace `path` with the actual paths to your `logzio-daemonset-containerd.yaml` and `configmap.yaml` files.


#####  Check Logz.io for your logs

Give your logs some time to get from your system to ours,
and then open [Kibana](https://app.logz.io/#/dashboard/kibana).

If you still don't see your logs,
see [log shipping troubleshooting](https://docs.logz.io/user-guide/log-shipping/log-shipping-troubleshooting.html).

</div>

### Disabling systemd input

To suppress Fluentd system messages, set the environment variable `FLUENTD_SYSTEMD_CONF` to `disable` in your Kubernetes environment.

### Disabling Prometheus input plugins

By default, the latest images launch `prometheus` plugins to monitor Fluentd.
If you'd like to disable the Prometheus input plugin, set the environment variable `FLUENTD_PROMETHEUS_CONF` to `disable` in your Kubernetes configuration.

### Exclude logs from certain namespaces

If you wish to exclude logs from certain namespaces, add the following to your Fluentd configuration:

```shell
<match kubernetes.var.log.containers.**_NAMESPACE_**>
  @type null
</match>
```

Replace `NAMESPACE` with the name of the namespace you need to exclude logs from. 
  
If you need to specify multiple namespaces, add another `kubernetes.var.log.containers.**_NAMESPACE_**` line to the above function as follows:

```shell
<match kubernetes.var.log.containers.**_NAMESPACE1_** kubernetes.var.log.containers.**_NAMESPACE2_**>
  @type null
</match>
```

</div>
<!-- tab:end -->



<!-- tab:start -->
<div id="multiline">


{% include /log-shipping/multiline-fluentd-plugin.md %}

</div>
<!-- tab:end -->

<!-- tab:start -->
<div id="troubleshooting">

This section contains some guidelines for handling errors that you may encounter when trying to run this solution.

## Problem: /file/path.log unreadable

The following error appears when running Fluentd:

```shell
/file/path.log unreadable. it is excluded and would be examined next time
```

### Possible cause

You may need to add more volume and volume mount to your Daemonset.

### Suggested remedy

<div class="tasklist">


##### Check on which node your pod is running

Find out on which node your Fluentd pod with the errors is running. To do so, use this command:

```shell
kubectl -n <<NAMESPACE>> get pod <<FLUENTD-POD-NAME>> -owide
```
  
##### Connect to the node

Connect to the node you found in the previous step (ssh, etc...).

##### Find the log's path

1. Run the following command, to go to the logs directory:

```shell
cd /var/log/containers
```

2. Run the following command to display the log files symlinks:

```shell
ls -ltr
```

This command should present you a list of your log files and their symlinks, for example:

```shell
some-log-file.log -> /var/log/pods/file_name.log
```

3. Choose one of those logs, copy the symlink, and run the following command:

```shell
ls -ltr /var/log/pods/file_name.log
```

Again, this command will output the file and its symlink, or example:

```shell
/var/log/pods/file_name.log -> /some/other/path/file.log
```

This directory (`/some/other/path`) is the directory where your log files are mounted at the host. You'll need to add that path to your Daemonset.

##### Add the mount path to your Daemonset

1. Open your Daemonset in your preffered text editor.
2. In the `volumeMounts` section, add the following:

```yaml
- name: logextramount
  mountPath: <<MOUNT-PATH>>
  readOnly: true
```

Replace `<<MOUNT-PATH>>` with the directory path you found in step 3.

3. In the `volumes` section, add the following:

```yaml
- name: logextramount
  hostPath:
    path: <<MOUNT-PATH>>
```

Replace `<<MOUNT-PATH>>` with the directory path you found in step 3.

4. Save the changes.

##### Deploy your new Daemonset.

Remove your previous Daemonset from the cluster, and apply the new one.

<!-- info-box-start:info -->
Applying the new Daemonset without removing the old one will not apply the changes.
{:.info-box.note}
<!-- info-box-end -->



##### Check your Fluentd pods to ensure that the error is gone

```shell
kubectl -n <<NAMESPACE>> logs <<POD-NAME>>
```


</div>
<!-- tab:end -->

</div>
<!-- tabContainer:end -->
