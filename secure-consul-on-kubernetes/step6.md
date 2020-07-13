In the previous section of the lab, you updated the development host configuration with
enough settings to allow you to list members of the Consul service mesh using the cli.

### ACL enforcement validation
Try inserting a value to the Key-Value store.

`consul kv put -ca-file consul-agent-ca.pem apple=banana`{{execute T1}}

This command fails with the following message:

`Error! Failed writing data: Unexpected response code: 403 (Permission denied)`

The 403 response is a sign that ACLs are being enforced. You have not yet supplied
an ACL token, and so the command fails. The `consul members` command worked because
consul-helm created an anoymous token and set the following policy for it:

```hcl
node_prefix "" {
   policy = "read"
}
service_prefix "" {
   policy = "read"
}
```

This policy is necessary for DNS, more specifically, so that the Kubernetes DNS server can
make DNS queries against Consul, so that queries like `consul members` end up working
without a client specifically providing a token. If you don't want to use Consul DNS,
you could disable it in the chart by setting `dns.enabled` to false. This will tell
the ACL bootstrapping job not to create a policy for the anonymous token. However, if
you want to use Consul DNS, this policy is required.

### Setting an ACL Token

The consul-helm chart created several secrets during the initialization process and
registered them with Kubernetes. For a list of all Kubernetes secrets run the following
command:

`kubectl get secrets`{{execute T1}}


Notice that one of the secrets is named `katacoda-consul-bootstrap-acl-token`. This
secret contains the Consul ACL [bootstrap token](https://learn.hashicorp.com/consul/day-0/acl-guide).
The bootstrap token is a full access token, that can perform any operation in the
service mesh. In a production scenario, you should avoid using the bootstrap token,
but for this lab you will use it for convenience.

To view the Kubernetes secret, execute the following command:

`kubectl get secret katacoda-consul-bootstrap-acl-token -o yaml | more`{{execute T1}}

You should recieve output similar to the following, though this example has been abbreviated.

```yaml
apiVersion: v1
data:
  token: OTk5ZGU1MGUtY2Q2Zi1iZGZiLTlkZmQtMDgwYjc0YTJjM2Jh
kind: Secret
metadata:
```

The value of interest is the string in the data stanza's token field. That value is a base64
encoded string that contains the bootstrap token generated during the consul-helm ACL init
process.

For this lab you can retrieve the value, decode it, and set it to the CONSUL_HTTP_TOKEN
environment variable by running the following command.

`export CONSUL_HTTP_TOKEN=$(kubectl get secrets/katacoda-consul-bootstrap-acl-token --template={{.data.token}} | base64 -d)`{{execute T1}}

Now, try to set a Key-Value store value again, with an ACL token set.

`consul kv put -ca-file consul-agent-ca.pem apple=banana`{{execute T1}}

The command succeeds. Now you have proven that ACLs are being enforced.
