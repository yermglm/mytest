# influxdb-grafana

[influxdb-grafana](https://github.com/kubernetes/charts/tree/master/stable/grafana) is an Grafana-Helm controller that uses InfluxDb.

## Install

```console
git clone http://192.168.201.22:30002/mi-banco-org/helm-charts.git

helm install ./helm-charts/helm-grafana --name grafana-chart -f ./helm-charts/helm-grafana/values.yaml \
  --namespace kube-system \
  --set server.service.type=NodePort \
  --set server.persistentVolume.enabled=true \
  --set dashboardImports.enabled=true \
  --set server.setDatasource.enabled=true \
  --set server.service.nodePort=30003 \
  --set server.service.name=monitoring-grafana \
  --set server.image=localhost:30004/grafana/grafana:4.6.3 \
  --set image.registryCredentials.username=registryUsr \
  --set image.registryCredentials.password=88BpJaZJDfn
```




## Update a previous installation

```console
helm upgrade chart \
  --namespace kube-system \
  --set server.service.type=NodePort \
  --set server.persistentVolume.enabled=true \
  --set dashboardImports.enabled=true \
  --set server.setDatasource.enabled=true \
  --set server.service.nodePort=30033 \
  --set server.image=192.168.201.22:30007/grafana/grafana:4.6.3
  ./influxdb-grafana
```

Where 

* `chart` is the Grafana release name
* `./influxdb-grafana` is the chart


## Introduction

This chart install deployment on a [Kubernetes](http://kubernetes.io) using the [Helm](https://helm.sh) package manager.

## Prerequisites
  - Kubernetes 1.4+ with Beta APIs enabled

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install --name my-release stable/grafana
```

The command deploys grafana on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`


## Uninstalling Grafana and all Components

```console
helm del --purge my-release
```

> **Tip**: List all grafanausing `helm ls --all my-release`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Installing the Dashboards

To install the dashboards copy the json files from `/influxdb-grafana/dashboard-templates`:
to
`/local/share/grafana/dashboards`

import via UID:

```console
- Go to dashboard/import
- select upload .json file.
- select influxdb-datasource
- click on import.
```

## Configuration

The following tables lists the configurable parameters of the grafana chart and their default values.

| Parameter                                 | Description                         | Default                                           |
|-------------------------------------------|-------------------------------------|---------------------------------------------------|
| `server.image`                            | Container image to run              | grafana/grafana:4.6.3                             |
| `server.adminUser`                        | Admin user username                 | admin                                             |
| `server.adminPassword`                    | Admin user password                 | Randomly generated                                |
| `server.antiAffinity.enabled`             | Enable anti affinity                | false                                             |
| `server.antiAffinity.type`                | Hard or soft anti affinity          | hard                                              |
| `server.persistentVolume.enabled`         | Create a volume to store data       | true                                              |
| `server.persistentVolume.size`            | Size of persistent volume claim     | 1Gi RW                                            |
| `server.persistentVolume.storageClass`    | Type of persistent volume claim     | `nil` (uses alpha storage class annotation)       |
| `server.persistentVolume.accessMode`      | ReadWriteOnce or ReadOnly           | [ReadWriteOnce]                                   |
| `server.persistentVolume.existingClaim`   | Existing persistent volume claim    | null                                              |
| `server.persistentVolume.subPath`         | Subdirectory of pvc to mount        | null                                              |
| `server.readinessProbe`                   | Server readiness probe              | httpGet: {path: /api/health, port: 3000}, initialDelaySeconds: 30, timeoutSeconds: 30 |
| `server.replicaCount`                     | Desired number of grafana pods      | 1                                                 |
| `server.resources`                        | Server resource requests and limits | requests: {cpu: 100m, memory: 100Mi}              |
| `server.tolerations`                      | node taints to tolerate (requires Kubernetes >=1.6) | null |
| `server.service.annotations`              | Service annotations                 | null                                              |
| `server.service.httpPort`                 | Service port                        | 80                                                |
| `server.service.loadBalancerIP`           | IP to assign to load balancer       | null                                              |
| `server.service.loadBalancerSourceRanges` | List of IP CIDRs allowed access     | null                                              |
| `server.service.nodePort`                 | For service type "NodePort"         | null                                              |
| `server.service.externalIPs`              | External IP addresses               | null                                              |
| `server.service.clusterIP`                | Custom clusterIP to use for service | null                                         |
| `server.service.type`                     | ClusterIP, NodePort, or LoadBalancer| ClusterIP                                         |
| `server.setDatasource.enabled`            | Creates grafana datasource with job | false                                             |
| `server.extraEnv`                          | Extra environment variables to set in the server container. List of [EnvVar](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.9/#envvar-v1-core) values | [] |
| `dashboardImports.enabled`                | Creates grafana dashboards with job | false |
`imagePullSecrets` | Specify image pull secrets | pull-regitry-keys


Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```console
$ helm install ./influxdb-grafana --name my-release -f values.yaml
```
