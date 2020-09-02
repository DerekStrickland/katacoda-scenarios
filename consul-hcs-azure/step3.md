Next, configure the necessary secrets to enable communication
between the HCS cluster and the AKS cluster.

First, start by bootstrapping Consul ACLs and storing the token
as a Kubernetes secret.

`az hcs create-token --name $HCS_MANAGED_APP --resource-group $RESOURCE_GROUP --output-kubernetes-secret | kubectl apply -f -`{{execute T1}}

Example output:

```plaintext
secret/dwcc-username-managed-hcs-bootstrap-token created
```

Next, generate a Kubernetes secret with credentials for the HCS cluster.

`az hcs generate-kubernetes-secret --name $HCS_MANAGED_APP --resource-group $RESOURCE_GROUP | kubectl apply -f -`{{execute T1}}

Example output:

```plaintext
secret/dwcc-username-managed-hcs created
```

Next, generate the helm configuration file that you will apply to your AKS cluster.

`az hcs generate-helm-values --name $HCS_MANAGED_APP --resource-group $RESOURCE_GROUP --aks-cluster-name $AKS_CLUSTER > config.yaml`{{execute T1}}

Now, open `config.yaml`{{open}} and uncomment line 29 so that gossip ports are exposed.

```yaml
  # If you are using Kubenet in your AKS cluster (the default network),
  # uncomment the line below.
  exposeGossipPorts: true
```
