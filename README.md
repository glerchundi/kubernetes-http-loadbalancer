# kubernetes-http-loadbalancer
a complete working example of a HTTP loadbalancer using nginx & kubernetes.

It uses:

* [`nginx-loadbalancer`](https://github.com/just-containers/nginx-loadbalancer): nginx + confd based loadbalancer (in this case etcd is used as a backend).
* [`loadbalancer-feeder`](https://github.com/just-containers/loadbalancer-feeder): [`kubelistener`](https://github.com/glerchundi/kubelistener) listens to kubernetes pod events and changes etcd accordingly following our loadbalancer rules. 
* [`helloworld`](https://github.com/glerchundi/docker-helloworld): It renders the `'Hello from <hostname>'` message as a http response in order to identify if it's actually doing the load balancing.

```
$> etcdctl ls --recursive /lb
/lb/hosts
/lb/hosts/lisa.contoso.com
/lb/hosts/lisa.contoso.com/listeners
/lb/hosts/lisa.contoso.com/listeners/ls1 = {"protocol":"http","address":"0.0.0.0:80"}
/lb/hosts/lisa.contoso.com/locations 
/lb/hosts/lisa.contoso.com/locations/loc1 = {"path":"/","upstream":"helloworld-"}
/lb/upstreams
/lb/upstreams/helloworld-
/lb/upstreams/helloworld-/servers
/lb/upstreams/helloworld-/servers/eb636d5d-f8c0-11e4-b35e-080027520379 = {"url":"10.244.49.30:8080"}
/lb/upstreams/helloworld-/servers/eb63c855-f8c0-11e4-b35e-080027520379 = {"url":"10.244.49.31:8080"}
/lb/upstreams/helloworld-/servers/eb63e9d7-f8c0-11e4-b35e-080027520379 = {"url":"10.244.49.32:8080"}
```

**Note**: No! `-` is not a typo error, it's the `generateName` parameter if pod was created using a replication controller :-).
