In the previous section of the lab, you updated the development host configuration with
enough settings to allow you to list members of the Consul service mesh using the cli.

### ACL enforcement validation

Next, try inserting a value to the Key-Value store.

`consul kv put -ca-file consul-agent-ca.pem password=B@n@n@!!`{{execute T1}}

This command fails with the following message:

`Error! Failed writing data: Unexpected response code: 403 (Permission denied)`

The 403 response is a sign that ACLs are being enforced. You have not yet supplied
an ACL token, and so the command fails. This proves ACLs are being enforced.

### Setting an ACL Token

List all Kubernetes secrets run the following command:

`kubectl get secrets`{{execute T1}}

Notice that one of the secrets is named `katacoda-consul-bootstrap-acl-token`. This
secret contains the Consul ACL bootstrap token.

Run the following command to etrieve the token value, decode it, and set it to the CONSUL_HTTP_TOKEN
environment variable.

`export CONSUL_HTTP_TOKEN=$(kubectl get secrets/katacoda-consul-bootstrap-acl-token --template={{.data.token}} | base64 -d)`{{execute interrupt T1}}

Now, try to set a Key-Value store value again, with an ACL token set.

`consul kv put -ca-file consul-agent-ca.pem password=B@n@n@!!`{{execute T1}}

The command succeeds. Now you have proven that ACLs are being enforced.
