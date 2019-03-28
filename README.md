# polyaxon-nfs-provisioner

Polyaxon in-cluster NFS provisioner to simplify the creation of ReadWriteMany and ReadOnlyMany volumes.


## Description

This repo aims to provide a stable Helm chart, maintained and supported by Polyaxon, to easily deploy and spin NFS-volumes to use with Polyaxon.

The chart provides the possibility to easily create 5 volumes: `data`, `outputs`, `repos`, `logs`, and `upload`. 

By default the volumes are disabled, and the chart can be used to create as many volumes.

## Install

### Setup Helm

To install the TFJob operator make sure you have helm installed, please see this [guide](https://docs.polyaxon.com/guides/setup-helm/).

### Namespace

If you are using this chart with Polyaxon, please install the chart on the same namespace where you installed Polyaxon.

```bash
$ kubectl create namespace polyaxon

namespace "polyaxon" created
```

### Polyaxon's charts repo

You can add the Polyaxon helm repository to your helm, so you can install Polyaxon and other charts provided by Polyaxon from it. 
This makes it easy to refer to the chart without having to use a long URL each time.

```bash
$ helm repo add polyaxon https://charts.polyaxon.com
$ helm repo update
```

### Install the nfs provisioner

```bash
helm install polyaxon/nfs-provisione --name=plxtf --namespace=polyaxon
```


## Configuration

The helm chart provides a simple interface for creating volumes. By default these volumes are disabled, to enable them:

```yaml
logs:
  enabled: true

repos:
  enabled: true

upload:
  enabled: true

data:
  enabled: true

outputs:
  enabled: true
```

You can also enable only one or some of the volumes.

> N.B.1 Each volume has a default name prefixed by `polyaxon-pvc`, you can change that easily.

> N.B.2 The chart will create a provisioner with a `storageClass` named: `polyaxon-nfs`.

## Creating other volumes than those provided in values

You can create as many volumes as you once the provisioner is deployed on your cluster, you just need to provide a PVC name, storage size, and an access Mode:

```yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: polyaxon-pvc-foo
spec:
  accessModes:
    - ReadOnlyMany
  resources:
    requests:
      storage: 100Gi
  storageClassName: "polyaxon-nfs" 
```

