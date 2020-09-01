You will need to configure an ingress gateway to use demo application.

An ingress gateways in Consul belongs to a class of resources called
config entries. Open `assets/ingress-gateway.hcl`{{open}} to review
a baseline ingress gateway config entry. Specifically, note
that the ingress gateway is configured to expose port `8080` to external
traffic.

Now, register the config entry with Consul.

`consul config write assets/ingress-gateway.hcl`{{execute T1}}

To register the ingress gateway with Kubernetes, you must
update the `config.yaml`{{open}}, add a top level `ingressGateways` stanza
as shown below.

```yaml
ingressGateways:
  enabled: true
  gateways:
    - name: ingress-gateway
      service:
        type: LoadBalancer
```

Now, use `helm upgrade` to apply the updated `config.yaml` file.

`helm upgrade -f ./config.yaml hcs hashicorp/consul --version 0.24.1 --wait`{{execute T1}}

Example output:

```plaintext
add output
```