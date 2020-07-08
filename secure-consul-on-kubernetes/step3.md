# prove that ACLs are not enforced

`kubectl port-forward --address 0.0.0.0 katacoda-consul-server-0 8500:8500`{{execute T3}}

`consul members`{{execute T2}}

