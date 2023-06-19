# Configuring Suricata on Kubernetes

What we want to do is to support each other in the project of [jasonish/suricata](https://github.com/jasonish/docker-suricata).

## Kubernetes configuration

We have the configuration files as follows:

- [suricata-deployment.yaml](suricata-deployment.yaml)
- [suricata-service.yaml](suricata-service.yaml)

To create the deployment with the configuration file [``suricata-deployment.yaml``](suricata-deployment.yaml) on our machine we use the following:

``` bash
kubectl create -f suricata-deployment.yaml
```

> This will create a deployment with a single pod running the Suricata container with the necessary network capabilities and privileges.

If we want to see the Suricata logs, we can use the following command:

``` bash
kubectl logs <POD_NAME>
```

o

``` bash
kubectl logs -f <POD_NAME>
```

## Suircata configuration
