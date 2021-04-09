
### Question 1
Get the Pods from all namespaces.

Write down its content.
$ kubectl get pods --all-namespaces

NAMESPACE     NAME                               READY   STATUS        RESTARTS   AGE  
kube-system   cilium-9zvnv                       1/1     Running       0          4h44m
kube-system   cilium-operator-84bdd6f7b6-2tw5m   1/1     Running       0          4h46m
kube-system   cilium-operator-84bdd6f7b6-drbsm   1/1     Running       0          4h46m
kube-system   cilium-pp8p8                       1/1     Running       0          4h43m
kube-system   cilium-zfwjr                       1/1     Running       0          4h44m
kube-system   coredns-55ff57f948-8qgxv           1/1     Running       0          4h46m
kube-system   coredns-55ff57f948-jpk6k           1/1     Running       0          4h46m
kube-system   csi-do-node-qllxg                  2/2     Running       0          4h44m
kube-system   csi-do-node-xds5h                  2/2     Running       0          4h44m
kube-system   csi-do-node-zqflf                  2/2     Running       0          4h43m
kube-system   do-node-agent-72vzp                1/1     Running       0          4h44m
kube-system   do-node-agent-8qkzc                1/1     Running       0          4h43m
PS E:\_DESKTOP\Curso Kubernetes\Módulo 21\Exercicios\Exercicios01> kubectl get pods --all-namespaces
NAMESPACE     NAME                               READY   STATUS    RESTARTS   AGE  
kube-system   cilium-9zvnv                       1/1     Running   0          4h44m
kube-system   cilium-operator-84bdd6f7b6-2tw5m   1/1     Running   0          4h46m
kube-system   cilium-operator-84bdd6f7b6-drbsm   1/1     Running   0          4h46m
kube-system   cilium-pp8p8                       1/1     Running   0          4h44m
kube-system   cilium-zfwjr                       1/1     Running   0          4h44m
kube-system   coredns-55ff57f948-8qgxv           1/1     Running   0          4h46m
kube-system   coredns-55ff57f948-jpk6k           1/1     Running   0          4h46m
kube-system   csi-do-node-qllxg                  2/2     Running   0          4h44m
kube-system   csi-do-node-xds5h                  2/2     Running   0          4h44m
kube-system   csi-do-node-zqflf                  2/2     Running   0          4h44m
kube-system   do-node-agent-72vzp                1/1     Running   0          4h44m
kube-system   do-node-agent-8qkzc                1/1     Running   0          4h44m
kube-system   do-node-agent-9766x                1/1     Running   0          4h44m
kube-system   kube-proxy-fqv6l                   1/1     Running   0          4h44m
kube-system   kube-proxy-l46d8                   1/1     Running   0          4h44m
kube-system   kube-proxy-s8rrt                   1/1     Running   0          4h44m

### Question 2
Create a namespace called ns-5.

Create a Pod named nginx with the image nginx:1.17.4.

Expose the container on port 80.

Write down the list of Endpoints for the current namespace.

$ kubectl get endpoints  -n ns-5
service-nginx   10.244.1.28:80   3s

### Question 3
Create a Pod named busybox with the image busybox:1.31.0.

From within the Pod busybox, retrieve the path / on the Pod nginx.

Write down its content.

#$ kubectl run busybox-1 --image=docker.io/library/busybox:latest  -n ns-5  -- sleep 3000
$ kubectl exec -it busybox-1  -- /bin/ash
$ / # wget service-nginx:8080
$ / # cat  index.html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
/ # ssh
/bin/ash: ssh: not found
/ # cat  index.html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>


### Question 4
Create a Service named entrypoint that targets the Pod nginx on port 80.

The Service should be of type NodePort.

Write down the YAML file for the entrypoint Service.

apiVersion: v1

kind: Service

metadata:
  name: service-nginx

spec:
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
  
  type: NodePort



### Question 5
Create a Deployment named haproxy.

The Deployment should:

uses the haproxy:2.0.7 image
exposes port 80
have 3 replicas
Expose the Deployment with a Service of type ClusterIP named ingress.

From within the Pod busybox, retrieve the path / on the Service ingress.

Write down its content.

$ kubectl logs   haproxy-bcc8f877c-qjs6t  -n ns-5 -v10
#Erro generalizado em haproxy

### Question 6
Write a Service of type ClusterIP that target Pods from the Deployment haproxy and the Pod nginx.

Write down the list of Endpoint for that Service.

$ kubectl get endpoints -n ns-5
NAME              ENDPOINTS        AGE
service-busybox   10.244.1.73:22   15s
service-nginx     10.244.2.70:80   14s