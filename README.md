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

``` bash
kubectl exec -it <POD_NAME> -- sh
```

Inside suricata we are going to create a small rule to see if it is configured and working. In suricata we are going to create a small rule to see if it is configured and working. We will have to modify the configuration of ``/etc/suricata/suricata.yaml`` to make the relevant changes.

For example:

1. We made the configuration for the suricata rules:

   - The ``default-rule-path`` section is where we will generate our rules. The ``default-log-dir`` section is where the logs generated by suricata will be stored. We can change these paths if we want.
  
      ``` yaml
      default-rule-path: /var/lib/suricata/rules
      ```
    
      ``` yaml
      default-log-dir: /var/log/suricata
      ```

    - The ``rule-files`` section is where we define the files we have with our rules that will be applied in suricata.
  
      ``` yaml
      rule-files:
        - suricata.rules # suricata rules
        - my-rules # my rules
      ```

   - It is also important to look at the ``af-packet`` section under ``- interface:``, to configure the interface where suricata will perform the analysis.
     
      ``` yaml
      af-packet:
        - interface: eth0
      ```
      
2. We create a file with our own rules for Suricata inside our ``default-rule-path`` (eg: ``my-rules``). In this file we will create our rules, a very basic and easy to implement rule would be for ICMP packets:
   
   ``` text
   alert icmp any any -> any any (msg: "ICMP Packet found"; sid:2000001; rev:1;)
   ```
   
3. We proceed to run the suricata service by defining the configuration file, the rules and the interface:

   ``` text
   suricata -c /etc/suricata/suricata.yaml -s /var/lib/suricata/rules/my-rules -i etho
   ```
   

Once inside, you are going to set up a basic rule to check that meerkat responds to us. For this we are going to use the Ubunu POD with the [``ubuntupod.yaml``](ubuntupod.yaml) file to perform a ping.

``` bash
kubectl apply -f ubuntupod.yaml
```

``` bash
kubectl exec -it ubuntu -- sh
```

``` bash
ping <IP_POD_ADDR>
```




