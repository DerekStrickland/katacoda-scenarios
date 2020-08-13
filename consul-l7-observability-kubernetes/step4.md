
Install Grafana with the following command.

`kubectl apply -f app`{{execute interrupt T1}}

You should receive output similar to the following:

```plaintext
service/frontend-service created
...TRUNCATED
deployment.apps/public-api created
```

### Verify the installation

Verify the installation using the following command:

`watch kubectl get pods`{{execute T1}}

Once all pods have a status of `Running` the installation is complete.

```plaintext
NAME                                                         READY   STATUS    RESTARTS   AGE
consul-9p9jf                                                 1/1     Running   0          122m
consul-connect-injector-webhook-deployment                   1/1     Running   0          122m
consul-server-0                                              1/1     Running   0          122m
frontend                                                     3/3     Running   0          120m
grafana                                                      3/3     Running   0          120m
postgres                                                     3/3     Running   0          120m
products-api                                                 3/3     Running   0          120m
prometheus-kube-state-metrics                                3/3     Running   0          122m
prometheus-node-exporter                                     3/3     Running   0          122m
prometheus-pushgateway                                       3/3     Running   0          122m
prometheus-server                                            4/4     Running   0          122m
public-api                                                   3/3     Running   0          120m
```

Next, in a different terminal, forward port `80` from the `frontend` pod hosting the UI to the development
host's port `8080`.

`kubectl port-forward deploy/frontend 8080:80`{{execute T2}}

You will receive the following output.

```plaintext
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
```

Now, open the HashiCups UI tab. If the UI doesn't load immediately,
wait a minute and try again. The app take some time to initialize even
after the pods are marked as running.
