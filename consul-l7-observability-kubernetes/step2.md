
Install Prometheus with the following command.

`helm install -f helm/prometheus-values.yaml prometheus stable/prometheus --version "11.7.0" --wait`{{execute interrupt T1}}

You should receive output similar to the following:

```plaintext
NAME: prometheus
...
https://prometheus.io/
```

### Verify the installation

Verify the installation using the following command:

`watch kubectl get pods`{{execute T1}}

Once all pods have a status of `Running` the installation is complete.

```plaintext
consul-9p9jf                                                 1/1     Running   0          43m
consul-connect-injector-webhook-deployment-b797f6fd4-2cwvm   1/1     Running   0          43m
consul-server-0                                              1/1     Running   0          43m
prometheus-kube-state-metrics-c65b87574-4zjb7                3/3     Running   0          42m
prometheus-node-exporter-cq7lb                               3/3     Running   0          42m
prometheus-pushgateway-697b8c46cd-rjq4q                      3/3     Running   0          42m
prometheus-server-64c7484778-hwtmg                           4/4     Running   0          42m
```