First, click below to login to the Azure CLI.

`az login`{{execute T1}}

You will receive output like the following.

```plaintext

```

Click on the link to open a new browser tab and login to Azure.

Next, click below to install the HCS extension for the Azure CLI.

`az extension add --source https://releases.hashicorp.com/hcs/0.2.1/hcs-0.2.1-py2.py3-none-any.whl`{{execute T1}}

Next, click below to create a resource group for this lab.

`az group create -l centralus -n learn-hcs-lab`{{execute T1}}

Finally, create and HCS datacenter on Azure.

`az hcs create -g learn-hcs-lab --name learnlab --datacenter-name learn-dc --email example@hashicorp.com --external-endpoint enabled`{{execute T1}}

