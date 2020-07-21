First, you will install an unsecured Consul service mesh in a Kubernetes cluster.

### Download Helm repo

Click the box below to download the Helm repo:

`helm repo add hashicorp https://helm.releases.hashicorp.com`{{execute T1}}

### Review a basic config file

Review the `dc1.yaml`{{open}} file.

### Apply the chart

Apply the chart using the following command. It will start the consul clients and servers
and provision a persistent disk. The install may take a minute or two to complete.

`helm install -f ./dc1.yaml katacoda hashicorp/consul --wait`{{execute T1}}

You should receive output similar to the following:

```plaintext
NAME: katacoda
LAST DEPLOYED: Wed Jul  8 15:56:47 2020

...omitted

  $ helm status katacoda
  $ helm get all katacoda
```

### Verify installation

Verify the installation using the following command:

`watch kubectl get pods`{{execute T1}}

Once all pods have a status of `Running` the installation is complete.

```plaintext
NAME                                                 READY   STATUS    RESTARTS   AGE
consul-7d4h2                                         1/1     Running   0          82s
consul-connect-injector-webhook-deployment           1/1     Running   0          94s
consul-server-0                                      1/1     Running   0          93s
```