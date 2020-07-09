Next, you will upgrade your Consul service mesh installation to enable the three
core components of Consul security: gossip encryption, TLS, and ACLs.

### Review a security enbabled config file

Next, you would modify your custom configuration file, but for this hands on lab,
the `secure-dc1.yaml`{{open}} file has been provided with all the necessary options.
Feel free to review it now. We encourage you to compare it to the dc1.yaml file you
installed earlier. For a complete reference of all possible configuration options,
review the official [documentation](https://www.consul.io/docs/k8s/helm).

### Create gossip encryption key

You should notice that the file now includes a `gossipEncryption` stanza.

```yaml
gossipEncryption:
    secretName: "consul-gossip-encryption-key"
    secretKey: "key"
```

The `secretName` and `secretKey` entries refer to a Kubernetes secret that can be
retrieved at runtime. Use the following command to register a gossip encryption key
as a Kubernetes secret that the helm chart can consume.

-> Note this command uses [consul keygen](https://www.consul.io/docs/commands/keygen.html)
to generate a valid key. See the [documentation](https://www.consul.io/docs/agent/options#_encrypt)
for key requirements.

`kubectl create secret generic consul-gossip-encryption-key --from-literal=key=$(consul keygen)`{{execute T1}}

### create root ca
`consul tls ca create`{{execute T1}}

### create cert secret
`kubectl create secret generic consul-ca-cert --from-file='tls.crt=./consul-agent-ca.pem'`{{execute T1}}

### create private key secret
`kubectl create secret generic consul-ca-key --from-file='tls.key=./consul-agent-ca-key.pem'`{{execute T1}}

### helm upgrade
`helm upgrade -f ./secure-dc1.yaml katacoda hashicorp/consul --wait`{{execute T1}}

### Get pods

`kubectl get pods`{{execute T1}}
