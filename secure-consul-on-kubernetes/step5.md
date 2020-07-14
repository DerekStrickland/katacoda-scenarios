### TLS Configuration

Next, you will set necessary configuration options now that TLS is enabled.
In a separate terminal, configure Kubernetes to forward port 8501 so that
you can interract with the Consul CLI from the development host. Note that
with TLS enabled, Consul uses port 8501 instead of 8500.

`kubectl port-forward --address 0.0.0.0 katacoda-consul-server-0 8500:8501`{{execute T3}}

You must set the CONSUL_HTTP_SSL environment variable to true on any host attempting
to execute Consul CLI commands. Do that now from the development host with the following command:

`export CONSUL_HTTP_SSL=true`{{execute T1}}

Now, execute `consul members`. Note you must provide Consul with a way to verify TLS
connections. In this example, you are providing the CA as a CLI option.

`consul members -ca-file consul-agent-ca.pem`{{execute T1}}

You now observe a list of all members of the service mesh. This
proves that TLS is being enforced.
