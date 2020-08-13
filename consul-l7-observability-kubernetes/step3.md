
Install Grafana with the following command.

`helm install -f helm/grafana-values.yaml grafana stable/grafana --version "5.3.6" --wait`{{execute interrupt T1}}

You should receive output similar to the following:

```plaintext
NAME: grafana
...
#################################################################################
######   WARNING: Persistence is disabled!!! You will lose your data when   #####
######            the Grafana pod is terminated.                            #####
#################################################################################
```

### Verify the installation

Verify the installation using the following command:

`watch kubectl get pods`{{execute T1}}

Once all pods have a status of `Running` the installation is complete.

```plaintext
NAME                                                         READY   STATUS    RESTARTS   AGE
consul-9p9jf                                                 1/1     Running   0          117m
consul-connect-injector-webhook-deployment-b797f6fd4-2cwvm   1/1     Running   0          117m
consul-server-0                                              1/1     Running   0          117m
grafana-7d6f454b75-pb687                                     3/3     Running   0          115m
prometheus-kube-state-metrics-c65b87574-4zjb7                3/3     Running   0          117m
prometheus-node-exporter-cq7lb                               3/3     Running   0          117m
prometheus-pushgateway-697b8c46cd-rjq4q                      3/3     Running   0          117m
prometheus-server-64c7484778-hwtmg                           4/4     Running   0          117m
```