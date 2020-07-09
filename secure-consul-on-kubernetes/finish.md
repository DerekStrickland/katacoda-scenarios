In this hands-on lab you learned how to secure a Consul service mesh running in Kubernetes.

Specifically you used the Consul Helm chart to:

- Installed Consul service mesh in a Kubernetes cluster
- Verified that gossip traffic was clear text
- Verified that API traffic was clear text
- Verified that ACLs were not enforced
- Upgraded your Consul datacenter to enable gossip encryption, TLS, and ACLs
- Verified that gossip traffic was now encrypted
- Verified that API traffic was now encrypted
- Verified that ACL tokens were now required

