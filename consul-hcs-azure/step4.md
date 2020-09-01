Now you will configure your development host so that you
can issue commands with the Consul CLI.

Set the `CONSUL_HTTP_ADDR` environment variable.

`export CONSUL_HTTP_ADDR=$(az hcs show --name $HCS_MANAGED_APP --resource-group $RESOURCE_GROUP | jq -r .properties.consulExternalEndpointUrl) && echo $CONSUL_HTTP_ADDR`{{execute T1}}

Example output:

```plaintext
https://11eaebe7-28cc-d041-894b-0242ac110006.consul.11eaebe6-ef56-82ce-945a-0242ac11000c.az.hashicorp.cloud
```

Set the `CONSUL_HTTP_TOKEN` environment variable to the bootstrap
token to allow full command line access. **Warning:** the production
best practices is to use specific tokens for specific jobs/operators.

`export CONSUL_HTTP_TOKEN=$(kubectl get secret $HCS_MANAGED_APP-bootstrap-token -o jsonpath={.data.token} | base64 -D) && echo $CONSUL_HTTP_TOKEN`

Example output:

```plaintext
1978b243-d3bb-bb56-c88c-50892003698f
```

Set the `CONSUL_HTTP_SSL_VERIFY` environment variable to false.

`export CONSUL_HTTP_SSL_VERIFY=false && echo $CONSUL_HTTP_SSL_VERIFY`{{execute T1}}

Example output:

```plaintext
false
```

Now, verify that your development host is configured correctly
to interact with your HCS Managed App.

`consul members`{{execute T1}}

Example output:

```plaintext
Node                                               Address        Status  Type    Build      Protocol  DC       Segment
11eaebe7-28cc-d041-894b-0242ac110006-vmss-1000000  10.0.0.4:8301  left    server  1.8.0+ent  2         westus2  <all>
11eaebe7-28cc-d041-894b-0242ac110006-vmss-2000000  10.0.0.5:8301  alive   server  1.8.0+ent  2         westus2  <all>
```