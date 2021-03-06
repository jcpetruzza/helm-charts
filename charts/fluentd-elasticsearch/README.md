# Fluentd Elasticsearch

- Installs [Fluentd](https://www.fluentd.org/) log forwarder.

## TL;DR

```console
helm install kiwigrid/fluentd-elasticsearch
```

## Introduction

This chart bootstraps a [Fluentd](https://www.fluentd.org/) daemonset on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.
It's meant to be a drop in replacement for fluentd-gcp on GKE which sends logs to Google's Stackdriver service, but can also be used in other places where logging to ElasticSearch is required.
The used Docker image also contains Google's detect exceptions (for Java multiline stacktraces), Prometheus exporter, Kubernetes metadata filter & Systemd plugins.

## Prerequisites

- Kubernetes 1.8+ with Beta APIs enabled

## Installing the Chart

To install the chart with the release name `my-release`:

```console
helm install --name my-release kiwigrid/fluentd-elasticsearch
```

The command deploys fluentd-elasticsearch on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the Fluentd elasticsearch chart and their default values.

| Parameter                                    | Description                                                                    | Default                                |
| -------------------------------------------- | ------------------------------------------------------------------------------ | -------------------------------------- |
| `affinity`                                   | Optional daemonset affinity                                                    | `{}`                                   |
| `annotations`                                | Optional daemonset annotations                                                 | `NULL`                                 |
| `podAnnotations`                             | Optional daemonset's pods annotations                                          | `NULL`                                 |
| `configMaps.useDefaults.systemConf`          | Use default system.conf                                                        | true                                   |
| `configMaps.useDefaults.containersInputConf` | Use default containers.input.conf                                              | true                                   |
| `configMaps.useDefaults.systemInputConf`     | Use default system.input.conf                                                  | true                                   |
| `configMaps.useDefaults.forwardInputConf`    | Use default forward.input.conf                                                 | true                                   |
| `configMaps.useDefaults.monitoringConf`      | Use default monitoring.conf                                                    | true                                   |
| `configMaps.useDefaults.outputConf`          | Use default output.conf                                                        | true                                   |
| `extraConfigMaps`                            | Add additional Configmap or overwrite disabled default                         | `{}`                                   |
| `awsSigningSidecar.enabled`                  | Enable AWS request signing sidecar                                             | `false`                                |
| `awsSigningSidecar.resources`                | AWS Sidecar resources                                                          | `{}`                                   |
| `awsSigningSidecar.network.port`             | AWS Sidecar exposure port                                                      | `8080`                                 |
| `awsSigningSidecar.network.address`          | AWS Sidecar listen address                                                     | `localhost`                            |
| `awsSigningSidecar.image.repository`         | AWS signing sidecar repository image                                           | `abutaha/aws-es-proxy`                 |
| `awsSigningSidecar.image.tag`                | AWS signing sidecar repository tag                                             | `0.9`                                  |
| `elasticsearch.auth.enabled`                 | Elasticsearch Auth enabled                                                     | `false`                                |
| `elasticsearch.auth.user`                    | Elasticsearch Auth User                                                        | `""`                                   |
| `elasticsearch.auth.password`                | Elasticsearch Auth Password                                                    | `""`                                   |
| `elasticsearch.bufferChunkLimit`             | Elasticsearch buffer chunk limit                                               | `2M`                                   |
| `elasticsearch.bufferQueueLimit`             | Elasticsearch buffer queue limit                                               | `8`                                    |
| `elasticsearch.host`                         | Elasticsearch Host                                                             | `elasticsearch-client`                 |
| `elasticsearch.logstashPrefix`               | Elasticsearch Logstash prefix                                                  | `logstash`                             |
| `elasticsearch.port`                         | Elasticsearch Port                                                             | `9200`                                 |
| `elasticsearch.path`                         | Elasticsearch Path                                                             | `""`                                   |
| `elasticsearch.scheme`                       | Elasticsearch scheme setting                                                   | `http`                                 |
| `elasticsearch.sslVerify`                    | Elasticsearch Auth SSL verify                                                  | `true`                                 |
| `elasticsearch.sslVersion`                   | Elasticsearch tls version setting                                              | `TLSv1_2`                              |
| `elasticsearch.outputType`                   | Elasticsearch output type                                                      | `elasticsearch`                        |
| `elasticsearch.typeName`                     | Elasticsearch type name                                                        | `_doc`                                 |
| `elasticsearch.logLevel`                     | Elasticsearch global log level                                                 | `info`                                 |
| `env`                                        | List of env vars that are added to the fluentd pods                            | `{}`                                   |
| `fluentdArgs`                                | Fluentd args                                                                   | `--no-supervisor -q`                   |
| `secret`                                     | List of env vars that are set from secrets and added to the fluentd pods       | `[]`                                   |
| `extraVolumeMounts`                          | Mount extra volume, required to mount ssl certificates when ES has tls enabled | `[]`                                   |
| `extraVolume`                                | Extra volume                                                                   | `[]`                                   |
| `hostLogDir.varLog`                          | Specify where fluentd can find var log                                         | `/var/log`                             |
| `hostLogDir.dockerContainers`                | Specify where fluentd can find logs for docker container                       | `/var/lib/docker/containers`           |
| `hostLogDir.libSystemdDir`                   | Specify where fluentd can find logs for lib Systemd                            | `/usr/lib64`                           |
| `image.repository`                           | Image                                                                          | `quay.io/fluentd_elasticsearch/fluentd`|
| `image.tag`                                  | Image tag                                                                      | `v3.0.0`                               |
| `image.pullPolicy`                           | Image pull policy                                                              | `IfNotPresent`                         |
| `image.pullSecrets`                          | Image pull secrets                                                             | ``                                     |
| `livenessProbe.enabled`                      | Whether to enable livenessProbe                                                | `true`                                 |
| `livenessProbe.initialDelaySeconds`          | livenessProbe initial delay seconds                                            | `600`                                  |
| `livenessProbe.periodSeconds`                | livenessProbe period seconds                                                   | `60`                                   |
| `livenessProbe.kind`                         | livenessProbe kind                                                             | `Set to a Linux compatible command`    |
| `nodeSelector`                               | Optional daemonset nodeSelector                                                | `{}`                                   |
| `podSecurityPolicy.annotations`              | Specify pod annotations in the pod security policy                             | `{}`                                   |
| `podSecurityPolicy.enabled`                  | Specify if a pod security policy must be created                               | `false`                                |
| `priorityClassName`                          | Optional PriorityClass for pods                                                | `""`                                   |
| `prometheusRule.enabled`                     | Whether to enable Prometheus prometheusRule                                    | `false`                                |
| `prometheusRule.prometheusNamespace`         | Namespace for prometheusRule                                                   | `monitoring`                           |
| `prometheusRule.labels`                      | Optional labels for prometheusRule                                             | `{}`                                   |
| `rbac.create`                                | RBAC                                                                           | `true`                                 |
| `resources.limits.cpu`                       | CPU limit                                                                      | `100m`                                 |
| `resources.limits.memory`                    | Memory limit                                                                   | `500Mi`                                |
| `resources.requests.cpu`                     | CPU request                                                                    | `100m`                                 |
| `resources.requests.memory`                  | Memory request                                                                 | `200Mi`                                |
| `service`                                    | Service definition                                                             | `{}`                                   |
| `service.ports`                              | List of service ports dict [{name:...}...]                                     | Not Set                                |
| `service.ports[].type`                       | Service type (ClusterIP/NodePort)                                              | `ClusterIP`                            |
| `service.ports[].name`                       | One of service ports name                                                      | Not Set                                |
| `service.ports[].port`                       | Service port                                                                   | Not Set                                |
| `service.ports[].nodePort`                   | NodePort port (when service.type is NodePort)                                  | Not Set                                |
| `service.ports[].protocol`                   | Service protocol(optional, can be TCP/UDP)                                     | Not Set                                |
| `serviceAccount.create`                      | Specifies whether a service account should be created.                         | `true`                                 |
| `serviceAccount.name`                        | Name of the service account.                                                   | `""`                                   |
| `serviceAccount.annotations`                 | Specify annotations in the pod service account                                 | `{}`                                   |
| `serviceMetric.enabled`                      | Generate the metric service regardless of whether serviceMonitor is enabled.   | `false`                                |
| `serviceMonitor.enabled`                     | Whether to enable Prometheus serviceMonitor                                    | `false`                                |
| `serviceMonitor.port`                        | Define on which port the ServiceMonitor should scrape                          | `24231`                                |
| `serviceMonitor.interval`                    | Interval at which metrics should be scraped                                    | `10s`                                  |
| `serviceMonitor.path`                        | Path for Metrics                                                               | `/metrics`                             |
| `serviceMonitor.labels`                      | Optional labels for serviceMonitor                                             | `{}`                                   |
| `serviceMonitor.metricRelabelings`           | Optional metric relabel configs to apply to samples before ingestion           | `[]`                                   |
| `serviceMonitor.relabelings`                 | Optional relabel configs to apply to samples before scraping                   | `[]`                                   |
| `serviceMonitor.type`                        | Optional the type of the metrics service                                       | `ClusterIP`                            |
| `tolerations`                                | Optional daemonset tolerations                                                 | `[]`                                   |
| `updateStrategy`                             | Optional daemonset update strategy                                             | `type: RollingUpdate`                  |
| `additionalPlugins`                          | Optional additionnal plugins to install when pod starts                        | `{}`                                   |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
helm install --name my-release kiwigrid/fluentd-elasticsearch
```

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
helm install --name my-release -f values.yaml kiwigrid/fluentd-elasticsearch
```

## Installation

### IBM IKS

For IBM IKS path `/var/log/pods` must be mounted, otherwise only kubelet logs would be available

```yaml
extraVolumeMounts: |
    - name: pods
      mountPath: /var/log/pods
      readOnly: true

extraVolumes: |
    - name: pods
      hostPath:
        path: "/var/log/pods"
        type: Directory
```

### AWS Elasticsearch Domains

AWS Elasticsearch requires requests to upload data to be signed using [AWS Signature V4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html). In order to support this, you can add `awsSigningSidecar: {enabled: true}` to your configuration. This results in a sidecar container being deployed that proxies all requests to your Elasticsearch domain and signs them appropriately.

## Upgrading

### From a version < 2.0.0

When you upgrade this chart you have to add the "--force" parameter to your helm upgrade command as there have been changes to the lables which makes a normal upgrade impossible.

### From a version &ge; 4.9.3 to version &ge; 5.0.0

When upgrading this chart you need to rename `livenessProbe.command` parameter to `livenessProbe.kind.exec.command` (only applicable if `livenessProbe.command` parameter was used).

### From a version &lt; 6.0.0 to version &ge; 6.0.0

When upgrading this chart  you have to perform updates for any system that
uses fluentd output from systemd logs, because now:

- field names have removed leading underscores (`_pid` becomes `pid`)
- field names from systemd are now lowercase (`PROCESS` becomes `process`)

This means any system that uses fluend output needs to be updated,
especially:

- in Kibana go to `Management > Index Patterns`, for each index click on
   `Refresh field list` icon
- fix renamed fields in other places - such as Kibana or Grafana, in items
  such as dashboards queries/vars/annotations

It is strongly suggested to set up temporarily new fluentd instance with output
to another elasticsearch index prefix to see the differences and then apply changes. The amount of fields altered can be noticeable and hard to list them all in this document.

Some dashboards can be easily fixed with sed:

```bash
cat dashboard.json | sed -e 's/_PID/pid/g'
```

Below list of most commonly used systemd fields:

```text
__MONOTONIC_TIMESTAMP
__REALTIME_TIMESTAMP
_BOOT_ID
_CAP_EFFECTIVE
_CMDLINE
_COMM
_EXE
_GID
_HOSTNAME
_MACHINE_ID
_PID
_SOURCE_REALTIME_TIMESTAMP
_SYSTEMD_CGROUP
_SYSTEMD_SLICE
_SYSTEMD_UNIT
_TRANSPORT
_UID
CODE_FILE
CODE_FUNC
CODE_FUNCTION
CODE_LINE
MESSAGE
MESSAGE_ID
NM_LOG_DOMAINS
NM_LOG_LEVEL
PRIORITY
SYSLOG_FACILITY
SYSLOG_IDENTIFIER
SYSLOG_PID
TIMESTAMP_BOOTTIME
TIMESTAMP_MONOTONIC
UNIT
```
