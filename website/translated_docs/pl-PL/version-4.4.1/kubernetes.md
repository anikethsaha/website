---
id: version-4.4.1-kubernetes
title: Kubernetes
original_id: kubernetes
---

 Możesz znaleźć instrukcje wdrażania Verdaccio w grupie Kubernetes w magazynie [verdaccio/docker-example](https://github.com/verdaccio/docker-examples/tree/master/kubernetes-example). Jednakże, zalecana metoda do instalacji Verdaccio na Kubernetes grupie jest do użycia [Helm](https://helm.sh). Helm jest [Kubernetes](https://kubernetes.io) menedżerem pakietów, który przynosi wiele korzyści.

<div id="codefund">''</div>

## Helm

### Setup Helm

Jeśli nie używałeś wcześniej Helm, musisz ustawić kontroler Helm zwany Tiller:

```bash
helm init
```

### Install

Deploy the Helm [stable/verdaccio](https://github.com/kubernetes/charts/tree/master/stable/verdaccio) chart. In this example we use `npm` as release name:

```bash
helm install --name npm stable/verdaccio
```

### Deploy a specific version

```bash
helm install --name npm --set image.tag=2.6.5 stable/verdaccio
```

### Upgrading Verdaccio

```bash
helm upgrade npm stable/verdaccio
```

### Odinstalowywanie

```bash
helm del --purge npm
```

**Note:** this command delete all the resources, including packages that you may have previously published to the registry.


### Niestandardowa konfiguracja Verdaccio

You can customize the Verdaccio configuration using a Kubernetes *configMap*.

#### Prepare

Copy the [existing configuration](https://github.com/verdaccio/verdaccio/blob/master/conf/docker.yaml) and adapt it for your use case:

```bash
wget https://raw.githubusercontent.com/verdaccio/verdaccio/master/conf/docker.yaml -O config.yaml
```

**Note:** Make sure you are using the right path for the storage that is used for persistency:

```yaml
storage: /verdaccio/storage/data
auth:
  htpasswd:
    file: /verdaccio/storage/htpasswd
```

#### Deploy the configMap

Deploy the `configMap` to the cluster

```bash
kubectl create configmap verdaccio-config --from-file ./config.yaml
```

#### Deploy Verdaccio

Teraz możesz wdrożyć wykres Verdaccio Helm i określić konfigurację, której użyć:

```bash
helm install --name npm --set customConfigMap=verdaccio-config stable/verdaccio
```

## Rancher Support

[Rancher](http://rancher.com/) jest kompletnym kontenerem platformy zarządzania, który sprawia, że zarządzanie i używanie kontenerów w produkcji jest naprawdę łatwe.

* [verdaccio-rancher](https://github.com/lgaticaq/verdaccio-rancher)
