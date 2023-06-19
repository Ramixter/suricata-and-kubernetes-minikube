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

Once we have our deployment active, let's see what its IP address is:

``` bash
kubectl get pod <POD_NAME> -o wide 
```
and here we will have the IP address that we are going to be working with for the moment to test using ICMP packets with PING.

## Suircata configuration

Now in the suricata's part, what we are going to do is to make some slight configurations in order to check that it works correctly.


