In this hands-on lab you will learn how to secure a Consul service mesh running in Kubernetes.

Specifically you will use the Consul Helm chart to:

- Create an insecure Consul service mesh running inside of a Kubernetes cluster
- Verify that gossip traffic occurs in clear text
- Verify that API traffic occurs in clear text
- Verify that ACLs are not enforced
- Upgrade your Consul datacenter to enable gossip encryption, TLS, and ACLs
- Verify that gossip traffic is now encrypted
- Verify that API traffic is now encrypted
- Verify that ACL tokens are now required
