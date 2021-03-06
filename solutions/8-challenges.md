# Apply the ingress to the cluster
```
$ k apply -f 8-ingress.yaml
ingress.networking.k8s.io/k8s-workshop created
```

# Find the exposed IP-address and invoke it (hint: describe the ingress)
```
$ k get ingresses
NAME           HOSTS   ADDRESS        PORTS   AGE
k8s-workshop   *       192.168.64.3   80      27h
```
Open the browser and try this url:
`http://192.168.64.3`

# Explore the ingress in the Kubernetes Dashboard
```
$ minikube dashboard
```

# Use the `k describe ingress` to find the path and backends for your ingress
```
$ k get ingress
NAME                   HOSTS              ADDRESS        PORTS   AGE
k8s-workshop-ingress   k8-workshop.info   192.168.64.5   80      4m55s
$ k describe ingress
```

## Add second deploy and add it to the ingress

```
$ k run web2 --image=gcr.io/google-samples/hello-app:2.0 --port=8080
$ k expose deployment web2 --target-port=8080
```

# add this deploy to the ingress.yaml and apply it
```
- host: k8s-workshop-web2.local
      http:
        paths:
          - path: /
            backend:
              serviceName: web2
              servicePort: 8080
```

# find the two different `curl` commands to run to hit the two different deploys
