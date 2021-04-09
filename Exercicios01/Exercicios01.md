### Question 1
Get the Pods from all namespaces.

Write down its content.

$ kubectl get deployments --all-namespaces
NAMESPACE     NAME              READY   UP-TO-DATE   AVAILABLE   AGE 
default       nginx             0/1     1            0           104s
kube-system   cilium-operator   2/2     2            2           29m 
kube-system   coredns           2/2     2            2           29m



### Question 2
Create a namespace called ns-2.
$ kubectl create namespace ns-2

Create the following Pods in that namespace:

A pod called nginx1 with a single container running the nginx:1.17.9 image
A pod called nginx2 that has one container running the nginx:1.17.4 image
A pod called haproxy that has one container running the haproxy:2.0.7 image
All pods should have a label of type app=proxy.

Write down the output of kubectl get pods for the ns-2 namespace.
$ kubectl get pods -n ns-2
NAME                     READY   STATUS    RESTARTS   AGE
nginx1-9bd84cd96-wr5sc   1/1     Running   0          23m
nginx2-fc969bddd-6zz5d   1/1     Running   0          21m

OBS: não foi possível cria o pod do haproxy. Sempre dá erro na execução do pod e não importa a imagem usada.


### Question 3
Retrieve all labels for the running container in the ns-2 namespace.

Write down its content.
$ kubectl get pods -l app=proxy -n ns-2
NAME                     READY   STATUS    RESTARTS   AGE
nginx1-9bd84cd96-wr5sc   1/1     Running   0          27m
nginx2-fc969bddd-6zz5d   1/1     Running   0          25m


### Question 4
Add additional labels to the Pods:

nginx1 should have a label of type webserver=nginx and a label of type version=1
nginx2 should have a label of type webserver=nginx and a label of type version=2
haproxy should have a label of type webserver=haproxy and a label of type version=1
Write down the output of kubectl get pods for the ns-2 namespace.
$ kubectl get pods -n ns-2
NAME                      READY   STATUS    RESTARTS   AGE
nginx1-5c74dbfdf4-kqq4q   1/1     Running   0          10s
nginx2-7f7bcf495c-8s2p6   1/1     Running   0          30s


### Question 5
Annotate the Pods with the following annotations:

nginx1 should have an annotation of description="first deployment"
nginx2 should have an annotation of description="second deployment"

### Question 6
With a single command, retrieve the Pods with the label webserver=nginx.

Write down the command.

$ kubectl get pods -l webserver=nginx  -n ns-2
NAME                      READY   STATUS    RESTARTS   AGE
nginx1-7fb9d69c96-77kqf   1/1     Running   0          62s
nginx2-7bc4784fc-w6hhf    1/1     Running   0          62s

### Question 7
With a single command, retrieve the Pods with the label version=1.

Write down the command.

$ kubectl get pods -l version=1  -n ns-2
NAME                      READY   STATUS    RESTARTS   AGE 
nginx1-7fb9d69c96-77kqf   1/1     Running   0          110s


### Question 8
Retrieve the annotation for the haproxy Pod.

Write down its content.

Não consegui criar o POD em questão

### Question 9
Create a Deployment with the image nginx:1.17.4.

The Deployment should:

be called nginx
have 2 replicas
expose port 80
include the label webserver=nginx for all Pods
Write down the output of kubectl get pods and kuebctl get deployments for the ns-2 namespace.

$ kubectl get deployments  -n ns-2
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
nginx    2/2     2            2           40s
nginx1   1/1     1            1           16m
nginx2   1/1     1            1           16m


### Question 10
Write down the YAML file for the ReplicaSet that was created by the nginx Deployment.

$ kubectl get replicaset nginx-77f76fc677  -n ns-2  -o yaml

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  annotations:
    deployment.kubernetes.io/desired-replicas: "2"
    deployment.kubernetes.io/max-replicas: "3"
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: "2021-04-09T13:26:38Z"
  generation: 1
  labels:
    app: proxy
    pod-template-hash: 77f76fc677
    webserver: nginx
  managedFields:
  - apiVersion: apps/v1
    fieldsType: FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          .: {}
          f:deployment.kubernetes.io/desired-replicas: {}
          f:deployment.kubernetes.io/max-replicas: {}
          f:deployment.kubernetes.io/revision: {}
        f:labels:
          .: {}
          f:app: {}
          f:pod-template-hash: {}
          f:webserver: {}
        f:ownerReferences:
          .: {}
          k:{"uid":"5a6aac22-1432-4522-9514-9bdc70840fae"}:
            .: {}
            f:apiVersion: {}
            f:blockOwnerDeletion: {}
            f:controller: {}
            f:kind: {}
            f:name: {}
            f:uid: {}
      f:spec:
        f:replicas: {}
        f:selector: {}
        f:template:
          f:metadata:
            f:labels:
              .: {}
              f:app: {}
              f:pod-template-hash: {}
              f:webserver: {}
          f:spec:
            f:containers:
              k:{"name":"nginx"}:
                .: {}
                f:image: {}
                f:imagePullPolicy: {}
                f:name: {}
                f:ports:
                  .: {}
                  k:{"containerPort":80,"protocol":"TCP"}:
                    .: {}
                    f:containerPort: {}
                    f:protocol: {}
                f:resources:
                  .: {}
                  f:limits:
                    .: {}
                    f:cpu: {}
                    f:memory: {}
                f:terminationMessagePath: {}
                f:terminationMessagePolicy: {}
            f:dnsPolicy: {}
            f:restartPolicy: {}
            f:schedulerName: {}
            f:securityContext: {}
            f:terminationGracePeriodSeconds: {}
      f:status:
        f:availableReplicas: {}
        f:fullyLabeledReplicas: {}
        f:observedGeneration: {}
        f:readyReplicas: {}
        f:replicas: {}
    manager: kube-controller-manager
    operation: Update
    time: "2021-04-09T13:26:39Z"
  name: nginx-77f76fc677
  namespace: ns-2
  ownerReferences:
  - apiVersion: apps/v1
    blockOwnerDeletion: true
    controller: true
    kind: Deployment
    name: nginx
    uid: 5a6aac22-1432-4522-9514-9bdc70840fae
  resourceVersion: "13880"
  uid: fc9ca82f-472b-4444-819e-0e9de07ee576
spec:
  replicas: 2
  selector:
    matchLabels:
      app: proxy
      pod-template-hash: 77f76fc677
      webserver: nginx
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: proxy
        pod-template-hash: 77f76fc677
        webserver: nginx
    spec:
      containers:
      - image: nginx:1.17.4
        imagePullPolicy: IfNotPresent
        name: nginx
        ports:
        - containerPort: 80
          protocol: TCP
        resources:
          limits:
            cpu: 500m
            memory: 128Mi
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 2
  fullyLabeledReplicas: 2
  observedGeneration: 1
  readyReplicas: 2
  replicas: 2

### Question 11
Downgrade the nginx Deployment to use the nginx:1.16.1 image.

Write down the output of kubectl get replicaset for the ns-2 namespace.

$  kubectl get replicasets   -n ns-2
NAME                DESIRED   CURRENT   READY   AGE
nginx-6d66494965    2         2         2       19s
nginx-77f76fc677    0         0         0       9m2s
nginx1-7fb9d69c96   1         1         1       24m
nginx2-7bc4784fc    1         1         1       24m


### Question 12
Downgrade, again, the nginx Deployment to use the nginx:1.7.0 image.

Write down the status of the rollout.
$ kubectl rollout status deployment/nginx  -n ns-2
Waiting for deployment "nginx" rollout to finish: 1 out of 2 new replicas have been updated...

$ kubectl get deployment -n ns-2
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
3         <none>

### Question 13
Inspect the history of the nginx Deployment.

Rollback to version 1.

Write down the output of history command for the nginx Deployment in the ns-2 namespace.

$ kubectl rollout undo deployment nginx -n ns-2
deployment.apps/nginx rolled back

### Question 14
Scale the nginx Deployment to three replicas.

Write down the list of pods with label webserver=nginx.

$ kubectl get pods -l webserver=nginx -n ns-2
NAME                      READY   STATUS         RESTARTS   AGE    
nginx-6d66494965-c9cx2    1/1     Running        0          121m   
nginx-6d66494965-ch4x8    1/1     Running        0          121m   
nginx-6d66494965-cs8mn    1/1     Running        0          62s    
nginx-7f6fddd8c4-2q2b5    0/1     ErrImagePull   0          62s    
nginx1-7fb9d69c96-77kqf   1/1     Running        0          146m   
nginx2-7bc4784fc-w6hhf    1/1     Running        0          146m

