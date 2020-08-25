Next, set a unique discriminator value so that your resource group and hcs
datacenter will have unique names.

`export SESSION_ID=$(uuidgen)`{{execute T1}}

Next, click below to create a resource group for this lab.

`az group create -l centralus -n learn-hcs-lab-$SESSION_ID`{{execute T1}}

Next, accept the terms and conditions.

`az vm image terms accept --offer hcs-production --publisher hashicorp-4665790 --plan On-demand`{{execute T1}}

publisher='hashicorp-4665790' offer = 'hcs-production', sku = 'on-demand', Correlation Id: 'bb8debfd-0631-4940-a71a-85a886fbc81a'

Finally, create and HCS datacenter on Azure.

`az hcs create -g learn-hcs-lab-$SESSION_ID --name learnlab --datacenter-name learn-dc --email example@hashicorp.com --external-endpoint enabled`{{execute T1}}
