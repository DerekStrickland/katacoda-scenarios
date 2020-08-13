
Install Grafana with the following command.

`kubectl apply -f app`{{execute interrupt T1}}

You should receive output similar to the following:

```plaintext
service/frontend-service created
configmap/nginx-configmap created
deployment.apps/frontend created
service/products-api-service created
serviceaccount/products-api created
configmap/db-configmap created
deployment.apps/products-api created
service/postgres created
deployment.apps/postgres created
service/public-api-service created
deployment.apps/public-api created
```

### Verify the installation

Verify the installation using the following command:

`watch kubectl get pods`{{execute T1}}

Once all pods have a status of `Running` the installation is complete.

```plaintext
NAME                                                         READY   STATUS    RESTARTS   AGE
consul-9p9jf                                                 1/1     Running   0          122m
consul-connect-injector-webhook-deployment-b797f6fd4-2cwvm   1/1     Running   0          122m
consul-server-0                                              1/1     Running   0          122m
frontend-6454bd57f-v958p                                     3/3     Running   0          120m
grafana-7d6f454b75-pb687                                     3/3     Running   0          120m
postgres-55899dddd-5z4hx                                     3/3     Running   0          120m
products-api-9cd5bbb69-j8x69                                 3/3     Running   0          120m
prometheus-kube-state-metrics-c65b87574-4zjb7                3/3     Running   0          122m
prometheus-node-exporter-cq7lb                               3/3     Running   0          122m
prometheus-pushgateway-697b8c46cd-rjq4q                      3/3     Running   0          122m
prometheus-server-64c7484778-hwtmg                           4/4     Running   0          122m
public-api-5f698b886-rfnh7                                   3/3     Running   0          120m
```

Next, in a different terminal, forward port `80` from the `frontend` pod hosting the UI to the development
host's port `8080`.

`kubectl port-forward deploy/frontend 8080:80`{{execute T2}}

You will receive the following output.

```plaintext
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
```

Now, open a new tab and view port 8080. If the UI doesn't show immediately,
wait a minute and try again. The app take some time to initialize even
after the pods are marked as running.
