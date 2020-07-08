# create gossip encryption key
`kubectl create secret generic consul-gossip-encryption-key --from-literal=key=$(consul keygen)`{{execute T1}}

# create root ca
`consul tls ca create`{{execute T1}}

# create cert secret
`kubectl create secret generic consul-ca-cert --from-file='tls.crt=./consul-agent-ca.pem'`{{execute T1}}

# create private key secret
`kubectl create secret generic consul-ca-key --from-file='tls.key=./consul-agent-ca-key.pem'`{{execute T1}}

# helm upgrade
`helm upgrade -f ./secure-dc1.yaml katacoda hashicorp/consul --wait`{{execute T1}}

# Get pods

`kubectl get pods`{{execute T1}}
