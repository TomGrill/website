---
id: version-4.4.1-kubernetes
title: Kubernetes
original_id: kubernetes
---

 Você pode encontrar instruções para implantar o Verdaccio em um cluster do Kubernetes no repositório [verdaccio/docker-example](https://github.com/verdaccio/docker-examples/tree/master/kubernetes-example). No entanto, o método recomendado para instalar o Verdaccio em um cluster de Kubernetes é usando [Helm](https://helm.sh). Helm é um gerenciador de pacotes do [Kubernetes](https://kubernetes.io) que traz múltiplas vantagens.

<div id="codefund">''</div>

## Helm

### Configurando o Helm

Se você nunca usou Helm antes, você precisará configurar o controlador do Helm chamado Tiller:

```bash
helm init
```

### Instalação

Deploy the Helm [stable/verdaccio](https://github.com/kubernetes/charts/tree/master/stable/verdaccio) chart. In this example we use `npm` as release name:

```bash
helm install --name npm stable/verdaccio
```

### Implemente uma versão específica

```bash
helm install --name npm --set image.tag=2.6.5 stable/verdaccio
```

### Atualizando o Verdaccio

```bash
helm upgrade npm stable/verdaccio
```

### Desinstalando

```bash
helm del --purge npm
```

**Note:** this command delete all the resources, including packages that you may have previously published to the registry.


### Configuração personalizada do Verdaccio

You can customize the Verdaccio configuration using a Kubernetes *configMap*.

#### Preparo

Copie a [configuração existente](https://github.com/verdaccio/verdaccio/blob/master/conf/docker.yaml) e adapte-a para o seu caso:

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

#### Implementando o configMap

Implemente o `configMap` no cluster

```bash
kubectl create configmap verdaccio-config --from-file ./config.yaml
```

#### Lançando Verdaccio

Agora você pode lançar a tabela Verdaccio Helm e especificar qual configuração usar:

```bash
helm install --name npm --set customConfigMap=verdaccio-config stable/verdaccio
```

## Suporte Rancher

[Rancher](http://rancher.com/) é uma plataforma completa de gerenciamento de contêineres que facilita o gerenciamento e o uso de contêineres na produção.

* [verdaccio-rancher](https://github.com/lgaticaq/verdaccio-rancher)
