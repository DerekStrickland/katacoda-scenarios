In this hands-on lab, you will learn how to deploy and secure a Consul service mesh running in Kubernetes.
This hands-on lab is designed to introduce you to the three cornerstones of Consul
service mesh security, and how they can be configured using the [Consul Helm chart](https://www.consul.io/docs/k8s/helm).

The three core components of Consul service mesh security are [Gossip Encryption](https://learn.hashicorp.com/consul/security-networking/agent-encryption),
[RPC encryption with TLS](https://learn.hashicorp.com/consul/security-networking/certificates), and [Access Controls Lists or ACLs](https://learn.hashicorp.com/consul/security-networking/production-acls). This hands-on lab will guide you through enabling all three using the Consul Helm chart.

Specifically, you will:

- Create an insecure Consul service mesh running inside of a Kubernetes cluster
- Verify that gossip traffic occurs in clear text
- Verify that API traffic occurs in clear text
- Verify that ACLs are not enforced
- Upgrade your Consul datacenter to enable gossip encryption, TLS, and ACLs
- Verify that gossip traffic is now encrypted
- Verify that API traffic is now encrypted
- Verify that ACL tokens are now required
