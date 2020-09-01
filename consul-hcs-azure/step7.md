Next, you must enable intentions between each service
and it's upstream starting with the ingress-gateway
you configured in the last step. Issue the following
command to add all necessary intentions.

`consul intention create ingress-gateway frontend && \
consul intention create frontend public-api && \
consul intention create public-api products-api && \
consul intention create products-api postgres`{{execute T1}}

Example output:

```plaintext
Created: ingress-gateway => frontend (allow)
Created: frontend => public-api (allow)
Created: public-api => products-api (allow)
Created: products-api => postgres (allow)
```

Now retrieve a list of services.

`kubectl get svc`{{execute T1}}

Example output:

```plaintext
NAME                          TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)                         AGE
consul-connect-injector-svc   ClusterIP      10.0.53.153   <none>           443/TCP                         4h43m
consul-ingress-gateway        LoadBalancer   10.0.55.177   40.125.122.123   8080:32296/TCP,8443:30997/TCP   76m
frontend                      ClusterIP      10.0.106.74   <none>           80/TCP                          4h39m
kubernetes                    ClusterIP      10.0.0.1      <none>           443/TCP                         22h
postgres                      ClusterIP      10.0.219.91   <none>           5432/TCP                        4h39m
products-api-service          ClusterIP      10.0.55.116   <none>           9090/TCP                        4h39m
public-api                    ClusterIP      10.0.13.8     <none>           8080/TCP                        4h39m
```

Notice the consul-ingress-gateway has an external ip
and that port `8080` is enabled.

Set the `INGRESS_IP` environment variable.

`export INGRESS_IP=$(kub get svc/consul-ingress-gateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}') && echo $INGRESS_IP`{{execute T1}}

Example output:

```plaintext
40.125.122.123
```

Set the `INGRESS_PORT` environment variable.

`export INGRESS_PORT=$(kub get svc/consul-ingress-gateway -o jsonpath='{.spec.ports[0].port}') && echo $INGRESS_PORT`{{execute T1}}

Example output:

```plaintext
40.125.122.123
```

Now, click on the HashiCups UI tab next to the console to visit the UI.