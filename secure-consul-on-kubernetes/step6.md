### ACL enforcement validation

Try listing Consul services.

`consul catalog services -ca-file ca.pem`{{execute T1}}

This command fails with the following message:

`Error! Failed writing data: Unexpected response code: 403 (Permission denied)`

You have not yet supplied an ACL token so the command fails.
This proves ACLs are being enforced.

### Setting an ACL Token

Run the following command to set the `CONSUL_HTTP_TOKEN`
environment variable from the Kubernetes `consul-bootstrap-acl-token`
secret.

`export CONSUL_HTTP_TOKEN=$(kubectl get secrets/consul-bootstrap-acl-token --template={{.data.token}} | base64 -d)`{{execute interrupt T1}}

Try to list services again.

`consul catalog services -ca-file ca.pem`{{execute T1}}

The command succeeds. You have proven that ACLs are being enforced.
