
# Download Helm repo

`helm repo add hashicorp https://helm.releases.hashicorp.com`{{execute T1}}

# Apply without gossip/tls/acls

`helm install -f ./assets/dc1.yaml katacoda hashicorp/consul --wait`{{execute T1}}

# Check pods

`kubectl get pods`{{execute T1}}


