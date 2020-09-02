Now, deploy Consul to the the AKS cluster using the `config.yaml` file you generated.

`helm install hcs hashicorp/consul -f config.yaml --version 0.24.1 --wait`{{execute T1}}

Abbreviated example output:

```plaintext
NAME: hcs
LAST DEPLOYED: Tue Sep  1 13:31:29 2020
NAMESPACE: default
...OMITTED
  $ helm status hcs
  $ helm get all hcs
```

Next, check that Consul is deployed and running.

`watch kubectl get pods`{{execute T1}}

The deployment is complete when all pods are Ready with a
status of `Running`.

```plaintext
NAME                                         READY   STATUS    RESTARTS   AGE
consul-5nmmx                                 1/1     Running   0          2m3s
consul-connect-injector-webhook-deployment   1/1     Running   0          2m3s
```

Once the deployment is complete, type CTRL-C to stop the watch.
Now, deploy a demo production workload to the AKS cluster.

`kubectl apply -f assets --wait`{{execute T1}}

Example output:

```plaintext
service/frontend created
serviceaccount/frontend created
configmap/nginx-configmap created
deployment.apps/frontend created
...OMITTED
```

Next, check that the workload is deployed and running.

`watch kubectl get pods`{{execute T1}}

The deployment is complete when all pods are Ready with a
status of `Running`.

Example output:

```plaintext
NAME                                         READY   STATUS    RESTARTS   AGE
consul-5nmmx                                 1/1     Running   0          6m4s
consul-connect-injector-webhook-deployment   1/1     Running   0          6m4s
frontend                                     3/3     Running   0          2m5s
postgres                                     3/3     Running   0          2m4s
products-api                                 3/3     Running   0          2m5s
public-api                                   3/3     Running   0          2m4s
```

Once the deployment is complete, type CTRL-C to stop the watch.