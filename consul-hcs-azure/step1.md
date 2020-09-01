First, you need to login to the Azure CLI using the login
command provided by your TA. The command will be similar
to this example.

`az login --service-principal --username <appId> --password <password> --tenant <tenant>`

az login \
  --service-principal \
  --username $(jq -r .appId sp.json) \
  --password $(jq -r .password sp.json) \
  --tenant $(jq -r .tenant sp.json)

You will receive output like the following.

```plaintext
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": REDACTED,
    "id": REDACTED,
    "isDefault": true,
    "managedByTenants": [
      {
        "tenantId": REDACTED
      }
    ],
    "name": REDACTED,
    "state": "Enabled",
    "tenantId": REDACTED,
    "user": {
      "name": REDACTED,
      "type": "servicePrincipal"
    }
```

## TODO - Not sure we need this for this lab
Next, click below to install the HCS extension for the Azure CLI. Enter `y` when prompted
to confirm installation.

`az extension add --source https://releases.hashicorp.com/hcs/0.2.1/hcs-0.2.1-py2.py3-none-any.whl`{{execute T1}}