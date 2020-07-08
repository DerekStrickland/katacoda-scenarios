
HashiCorp provides a first-class Consul on Kubernetes experience by means of a helm chart.
Installing Consul in Kubernetes consists of three steps:

- Downloading the consul-helm chart repository to the development host
- Defining your desired configuration in a yaml file
- Apply the chart to your cluster using the helm cli

## Download Helm repo

First, download the Helm repo run the following command:

`helm repo add hashicorp https://helm.releases.hashicorp.com`{{execute T1}}

## Review a basic config file

Next, you would define a custom configuration file, but for this sake of this hands on lab,
the `dc1.yaml`{{open}} file has been provided. Feel free to review it now. For a complete
reference of all possible configuration options, please review the official [documentation](https://www.consul.io/docs/k8s/helm)

## Apply the chart

Finally, apply the chart using the following command. Note that `katacoda` has been passed as an argument.
Helm requires a name be applied to each install. You may name your installation anything you like, but
in this tutorial 'katacoda' is used. When working locally, make sure to make note of the name of your installation,
as it will be required to apply future upgrades. The install may take a minute or two to complete.

`helm install -f ./dc1.yaml katacoda hashicorp/consul --wait`{{execute T1}}

When the installation is complete, you should receive output similar to the following:

`
NAME: katacoda
LAST DEPLOYED: Wed Jul  8 15:56:47 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
Thank you for installing HashiCorp Consul!

Now that you have deployed Consul, you should look over the docs on using
Consul with Kubernetes available here:

https://www.consul.io/docs/platform/k8s/index.html

Your release is named katacoda.

To learn more about the release if you are using Helm 2, run:

  $ helm status katacoda
  $ helm get katacoda

To learn more about the release if you are using Helm 3, run:

  $ helm status katacoda
  $ helm get all katacoda
`

## Verify installation

Once the installation is complete, you can verify everything was successful by reviewing the status
of running pods using the following command:

`kubectl get pods`{{execute T1}}

Once all pods are in a ready status, as illustrated in the following output, you can
proceed to the next step.

`
NAME                                                              READY   STATUS    RESTARTS   AGE
katacoda-consul-7d4h2                                             1/1     Running   0          82s
katacoda-consul-connect-injector-webhook-deployment-bd6c6dndk5b   1/1     Running   0          94s
katacoda-consul-server-0                                          1/1     Running   0          93s
`