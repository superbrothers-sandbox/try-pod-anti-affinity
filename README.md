# Try Pod Anti Affinity

Create a cluster which has one control-plane and three worker nodes:

```
$ kind create cluster --config config.yaml
```

Check your cluster information:

```
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"20", GitVersion:"v1.20.0", GitCommit:"af46c47ce925f4c4ad5cc8d1fca46c7b77d13b38", GitTreeState:"clean", BuildDate:"2020-12-08T17:59:43Z", GoVersion:"go1.15.5", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"20", GitVersion:"v1.20.2", GitCommit:"faecb196815e248d3ecfb03c680a4507229c2a56", GitTreeState:"clean", BuildDate:"2021-01-21T01:11:42Z", GoVersion:"go1.15.5", Compiler:"gc", Platform:"linux/amd64"}
$ kubectl get node
NAME                 STATUS   ROLES                  AGE     VERSION
kind-control-plane   Ready    control-plane,master   2m23s   v1.20.2
kind-worker          Ready    <none>                 111s    v1.20.2
kind-worker2         Ready    <none>                 111s    v1.20.2
kind-worker3         Ready    <none>                 111s    v1.20.2
```

Apply manifest files:

```
$ kubectl apply -f nginx-with-pod-anti-affinity.yaml -f nginx.yaml
deployment.apps/nginx-with-pod-anti-affinity created
deployment.apps/nginx created
```

Check which nodes Pods are scheduled on:

```
$ kubectl get po -o wide
NAME                                            READY   STATUS    RESTARTS   AGE   IP           NODE           NOMINATED NODE   READINESS GATES
nginx-55fcb8d94-ldkhm                           1/1     Running   0          20s   10.244.1.3   kind-worker3   <none>           <none>
nginx-55fcb8d94-lgmmf                           1/1     Running   0          20s   10.244.1.4   kind-worker3   <none>           <none>
nginx-55fcb8d94-zjv9c                           1/1     Running   0          20s   10.244.2.4   kind-worker    <none>           <none>
nginx-with-pod-anti-affinity-5fbc469b7d-47gz5   1/1     Running   0          5s    10.244.2.5   kind-worker    <none>           <none>
nginx-with-pod-anti-affinity-5fbc469b7d-d8n2h   1/1     Running   0          5s    10.244.3.5   kind-worker2   <none>           <none>
nginx-with-pod-anti-affinity-5fbc469b7d-mf2mf   1/1     Running   0          5s    10.244.1.5   kind-worker3   <none>           <none>
```
