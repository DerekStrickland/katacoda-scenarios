# Port forward from 8501 in cluster to 8500 on host

`kubectl port-forward katacoda-consul-server-0 8500:8501`{{execute interrupt T3}}

Note that we are using port 8501 on the container. Once TLS is enabled, Consul uses port 8501 for communications.

# Tell consul to use SSL

`consul members`{{execute T1}}

You should see an error

```shell
Unexpected response code: 400 (Client sent an HTTP request to an HTTPS server.)`
```

To force consul to use HTTPS set the CONSUL_HTTP_SSL environment variable to true.

`export CONSUL_HTTP_SSL=true`{{execute T1}}


# Tell consul to ignore SSL cert verification

`consul members`{{execute T1}}

Now you should see a new error

```shell
Error retrieving members: Get "https://127.0.0.1:8500/v1/agent/members?segment=_all": x509: certificate signed by unknown authority
```

{{execute T1}}

`export CONSUL_CA_FILE=consul-agent-ca.pem`{{execute T1}}

<!-- TODO:  What if I needed to verify?

`export CONSUL_HTTP_SSL_VERIFY=false`{{execute T1}} -->

# Try to view consul members

`consul members`{{execute T1}}

It fails with a 403 -- GOOD!!  You didn't supply a token
