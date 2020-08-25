First, click below to login to the Azure CLI.

`az login`{{execute T1}}

You will receive output like the following.

```plaintext
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code C8GXT8ST3 to authenticate.
```

Copy the code, and click on the link to open a new browser tab and login to Azure.

Next, click below to install the HCS extension for the Azure CLI. Enter `y` when prompted
to confirm installation.

`az extension add --source https://releases.hashicorp.com/hcs/0.2.1/hcs-0.2.1-py2.py3-none-any.whl`{{execute T1}}