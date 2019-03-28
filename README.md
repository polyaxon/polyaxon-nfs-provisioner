# polyaxon-nfs-provisioner

Polyaxon in-cluster NFS provisioner to simplify the creation of ReadWriteMany and ReadOnlyMany volumes.


## Description

This repo aims to provide a stable Helm chart, maintained and supported by Polyaxon, to easily deploy and spin NFS-volumes to use with Polyaxon.

The chart provides the possibility to easily create 5 volumes: `data`, `outputs`, `repos`, `logs`, and `upload`. 

By default the volumes are disabled, and the chart can be used to create as many volumes.

## Install

### Setup Helm

To install the nfs-provisioner, make sure you have helm installed, please see this [guide](https://docs.polyaxon.com/guides/setup-helm/).

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

## Reference

Polyaxon provides options to enable a built-in nfs provisioner to create some of the persistence storage needed or all of them.
In order to use this provisioner.

**logs**:

```yaml
logs:
  size: 10Gi
  enabled: true
```

| Parameter             | Description                                       | Default
| --------------------- | ------------------------------------------------- | ----------------------------------------------------------
| `logs.name`           | Name of the PVC to create                         | `polyaxon-pvc-logs`
| `logs.size`           | Size of data volume                               | `5Gi`
| `logs.mountPath`      | Path to mount the volume at, to use other image   | `/logs`
| `logs.accessMode`     | Use volume as ReadOnly or ReadWrite ReadWriteOnce | `ReadWriteMany`


**repos**:

```yaml
repos:
  size: 50Gi
  enabled: true
```

| Parameter              | Description                                       | Default
| ---------------------- | ------------------------------------------------- | ----------------------------------------------------------
| `repos.name`           | Name of the PVC to create                         | `polyaxon-pvc-repos`
| `repos.size`           | Size of data volume                               | `10Gi`
| `repos.mountPath`      | Path to mount the volume at, to use other image   | `/repos`
| `repos.accessMode`     | Use volume as ReadOnly or ReadWrite ReadWriteOnce | `ReadWriteMany`


**upload**:

```yaml
upload:
  size: 50Gi
  enabled: true
```

| Parameter               | Description                                       | Default
| ----------------------- | ------------------------------------------------- | ----------------------------------------------------------
| `upload.name`           | Name of the PVC to create                         | `polyaxon-pvc-upload`
| `upload.size`           | Size of data volume                               | `50Gi`
| `upload.mountPath`      | Path to mount the volume at, to use other image   | `/upload`
| `upload.accessMode`     | Use volume as ReadOnly or ReadWrite ReadWriteOnce | `ReadWriteMany`


**data**:

```yaml
data:
  size: 50Gi
  enabled: true
```

| Parameter             | Description                                       | Default
| --------------------- | ------------------------------------------------- | ----------------------------------------------------------
| `data.name`           | Name of the PVC to create                         | `polyaxon-pvc-data`
| `data.size`           | Size of data volume                               | `10Gi`
| `data.mountPath`      | Path to mount the volume at, to use other image   | `/data`
| `data.accessMode`     | Use volume as ReadOnly or ReadWrite ReadWriteOnce | `ReadWriteMany`


**outputs**:

```yaml
outputs:
  size: 50Gi
  enabled: true
```

| Parameter                | Description                                       | Default
| ------------------------ | ------------------------------------------------- | ----------------------------------------------------------
| `outputs.name`           | Name of the PVC to create                         | `polyaxon-pvc-outputs`
| `outputs.size`           | Size of data volume                               | `10Gi`
| `outputs.mountPath`      | Path to mount the volume at, to use other image   | `/outputs`
| `outputs.accessMode`     | Use volume as ReadOnly or ReadWrite ReadWriteOnce | `ReadWriteMany`
