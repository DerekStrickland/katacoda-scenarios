
Install Grafana with the following command.

`helm install -f helm/grafana-values.yaml grafana stable/grafana --version "5.3.6" --wait`{{execute interrupt T1}}

You should receive output similar to the following:

```plaintext
NAME: grafana
...TRUNCATED
########################
```

Verify the installation using the following command:

`watch kubectl get pods`{{execute T1}}

Once all pods have a status of `Running` the installation is complete.

```plaintext
NAME                                                        READY   STATUS    RESTARTS   AGE
consul-9p9jf                                                1/1     Running   0          117m
consul-connect-injector-webhook-deployment                  1/1     Running   0          117m
consul-server-0                                             1/1     Running   0          117m
grafana                                                     3/3     Running   0          115m
prometheus-kube-state-metrics                               3/3     Running   0          117m
prometheus-node-exporter                                    3/3     Running   0          117m
prometheus-pushgateway                                      3/3     Running   0          117m
prometheus-server                                           4/4     Running   0          117m
```
