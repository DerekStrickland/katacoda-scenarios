Next, upgrade your Consul service mesh installation to enable the three
core components of Consul security: gossip encryption, TLS, and ACLs.

### Review a security enbabled config file

Upgrading the service mesh is accomplished by updating the custom configuration
yaml file, and then running a `helm upgrade` command with the new configuration.
For this hands-on lab, the `secure-dc1.yaml`{{open}} file has been provided with
all the necessary options set. Feel free to review it now. We encourage you to
compare it to the `dc1.yaml`{{open}} file you installed earlier. For a complete
reference of all possible configuration options, review the official [documentation](https://www.consul.io/docs/k8s/helm).

### Configure Gossip encryption

If you haven't opened the `secure-dc1.yaml`{{open}} file, please do so now.
Notice that the file now includes a `gossipEncryption` stanza.

```yaml
gossipEncryption:
    secretName: "consul-gossip-encryption-key"
    secretKey: "key"
```

This entry provides the helm chart with the name of the Kubernetes secret to retrieve
and use as the gossip encryption key at runtime. You must create a valid key and register it
as a secret with Kubernetes.

The `secretName` and `secretKey` entries refer to a Kubernetes secret that can be
retrieved at runtime. Use the following command to register a gossip encryption key
as a Kubernetes secret that the helm chart can consume. *Note* this command uses
[consul keygen](https://www.consul.io/docs/commands/keygen.html) to generate a valid key.
See the [documentation](https://www.consul.io/docs/agent/options#_encrypt)
for key requirements.

`kubectl create secret generic consul-gossip-encryption-key --from-literal=key=$(consul keygen)`{{execute T1}}

### Configure TLS

TLS is enabled by the following stanza.

```yaml
tls:
    enabled: true
    enableAutoEncrypt: true
    verify: true
    caCert:
        secretName: consul-ca-cert
        secretKey: tls.crt
    caKey:
        secretName: consul-ca-key
        secretKey: tls.key
```

Notice that it also references two Kubernetes secrets that you will need to provide: a cert and a key.
To generate a CA run the following command:

`consul tls ca create`{{execute T1}}

This command generated consul-agent-ca.pem and consul-agent-ca-key.pem that you will register with Kubernetes.

`kubectl create secret generic consul-ca-cert --from-file='tls.crt=./consul-agent-ca.pem'`{{execute T1}}

`kubectl create secret generic consul-ca-key --from-file='tls.key=./consul-agent-ca-key.pem'`{{execute T1}}

You have now generated and registered all the necessary secrets to support both gossip encryption
and TLS.

### Configure managed ACLs

By setting manageSystemACLs to true, the ACL system will configure itself. You
do not need to take any action beyond setting the value in the config file.
The entry looks like this.

```yaml
acls:
    manageSystemACLs: true
```

### Helm upgrade

The config file for the chart has been properly configured, and all necessary secrets have
been registered with Kubernetes. Execute the following command to upgrade the installation
with these changes. The upgrade may take a minute or two to complete.

`helm upgrade -f ./secure-dc1.yaml katacoda hashicorp/consul --wait`{{execute T1}}

### Verify the upgrade

Once the upgrade is complete, you can verify everything was successful by reviewing the status
of running pods using the following command:

`kubectl get pods`{{execute T1}}

Once all pods are in a ready status, you can proceed to the next step.
