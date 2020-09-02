First, set an environment variable for the `username` your
TA provided you.  You can copy this command to your clipboard
and then paste it into the console and change the username.

`export USERNAME=username`{{copy}}

Next, set an environment variable to the name of your resource group.

`export RESOURCE_GROUP=dwcc-$USERNAME-rg && echo $RESOURCE_GROUP`{{execute T1}}

Example output:

```plaintext
dwcc-username-rg
```

Now, review all the resources in your environment.

`az resource list --resource-group $RESOURCE_GROUP | jq -r '.[] | .name'`{{execute T1}}

Error output:

```plaintext
The client 'REDACTED' with object id 'REDACTED' does not have authorization to perform action 'Microsoft.Resources/subscriptions/resourcegroups/read' over scope '/subscriptions/REDACTED/resourcegroups/dwcc-katacoda-rg' or the scope is invalid. If access was recently granted, please refresh your credentials.
```

Example output:

```plaintext
dwcc-username--aks
dwcc-username-peering
dwcc-username-managed-hcs
```

Next, set an environment variable to the name of your AKS cluster.

`export AKS_CLUSTER=$(az aks list --resource-group $RESOURCE_GROUP | jq -r '.[] | .name') && echo $AKS_CLUSTER`{{execute T1}}

Error output:

```plaintext
[]
```

Example output:

```plaintext
dwcc-username-aks
```

Next, export the Azure AKS KUBECONFIG settings to the development host.

`az aks get-credentials --name $AKS_CLUSTER --resource-group $RESOURCE_GROUP`{{execute T1}}

Error output:

```plaintext
The client 'REDACTED' with object id 'REDACTED' does not have authorization to perform action 'Microsoft.ContainerService/managedClusters/listClusterUserCredential/action' over scope '/subscriptions/REDACTED/resourceGroups/dwcc-katacoda-rg/providers/Microsoft.ContainerService/managedClusters/dwcc-katacoda-aks' or the scope is invalid. If access was recently granted, please refresh your credentials.
```

Example output:

```plaintext
Merged "dwcc-username-aks" as current context in /root/.kube/config
```

Verify the configuration by issuing the following command.

`kubectl get pods -n kube-system`{{execute T1}}

```plaintext
azure-cni-networkmonitor-8h9h8       1/1     Running   0          16h
azure-ip-masq-agent-xjkhf            1/1     Running   0          16h
coredns-869cb84759-4hwpj             1/1     Running   0          16h
coredns-869cb84759-l8f7t             1/1     Running   0          16h
coredns-autoscaler-5b867494f-5hphv   1/1     Running   0          16h
kube-proxy-8csmn                     1/1     Running   0          16h
metrics-server-6cd7558856-fzvz2      1/1     Running   0          16h
tunnelfront-76454d856b-hpcwb         2/2     Running   0          16h
```

Next, set an environment variable to the name of your HCS managed app.

`export HCS_MANAGED_APP=$(az hcs list --resource-group $RESOURCE_GROUP | jq -r '.[] | .name') && echo $HCS_MANAGED_APP`{{execute T1}}

Error output:

```plaintext
[]
```

Example output:

```plaintext
dwcc-username-managed-hcs
```