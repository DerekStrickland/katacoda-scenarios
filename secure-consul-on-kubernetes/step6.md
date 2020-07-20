### ACL enforcement validation

Try listing Consul services.

`consul catalog services`{{execute T1}}

This command fails with the following message:

`Error! Failed writing data: Unexpected response code: 403 (Permission denied)`

You have not yet supplied an ACL token so the command fails.
This proves ACLs are being enforced.

### Setting an ACL Token

List all Kubernetes secrets with the following command:

`kubectl get secrets`{{execute T1}}

One of the secrets is named `consul-bootstrap-acl-token`. This
secret contains the Consul ACL bootstrap token.

Run the following command to set the `CONSUL_HTTP_TOKEN`
environment variable from this secret.

`export CONSUL_HTTP_TOKEN=$(kubectl get secrets/consul-bootstrap-acl-token --template={{.data.token}} | base64 -d)`{{execute interrupt T1}}

Now export the CA file from the Kubernetes secrets store
so that you can pass it to the CLI.

`kubectl get secret consul-ca-cert -o jsonpath="{.data['tls\.crt']}" | base64 --decode > ca.pem`{{execute T1}}

Try list services again.

`consul catalog services -ca-file ca.pem`{{execute T1}}

The command succeeds. You have proven that ACLs are being enforced.
