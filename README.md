# kubernetes-labs

This is my repo for completing the Kubernetes Masterclass for Beginners 

## Section 1. Introduction & Setup
This sesction went over the basics and some tools to have setup for the course.

### Tools
Tools to install:
- Basics of command line navigation
-- workflow using `tmux`, `vim`, and `k9s`
- installing and configuring Rancher which is what will be used for this course.
- opening and closing files with vim
- Optional: git

### Setup
- Lab repo setup and Note taking
-- Note taking with Obsidian 
- MacOS
- Windows
- Linux

## Section 2: Kubernetes Fundementals
This section has some hands on examples and some code commits have been pushed.
- Develop a robust command-line interface (CLI) workflow using tmux, Vim, and k9s. This setup is also compatible with popular tools like VSCode, enhancing your productivity and efficiency.
- These tools are required to know for the CKA/CKAD exams
### overview of kubernetes cli tools
- What is Kubernetes and Why do we need it?
- Pods - Introduction
- Pods as Code - YAML
- Interacting with Pods

## Section 3: Deployments
This part of the lab is hands on running commands and what not.

The Events section of the output on a describe command is important for troubleshooting.

```
~/Git/udemy/kubernetes-labs • main ❯❯ kgp
NAME         READY   STATUS    RESTARTS   AGE     IP           NODE                   NOMINATED NODE   READINESS GATES
httpd        1/1     Running   0          3h21m   10.42.0.21   lima-rancher-desktop   <none>           <none>
nginx-docs   1/1     Running   0          3h31m   10.42.0.20   lima-rancher-desktop   <none>           <none>
nginx-yaml   1/1     Running   0          3h46m   10.42.0.19   lima-rancher-desktop   <none>           <none>
~/Git/udemy/kubernetes-labs • main ❯❯ k create deploy test --image httpd --replicas=3
deployment.apps/test created
~/Git/udemy/kubernetes-labs • main ❯❯ kgp
NAME                    READY   STATUS    RESTARTS   AGE     IP           NODE                   NOMINATED NODE   READINESS GATES
httpd                   1/1     Running   0          3h22m   10.42.0.21   lima-rancher-desktop   <none>           <none>
nginx-docs              1/1     Running   0          3h33m   10.42.0.20   lima-rancher-desktop   <none>           <none>
nginx-yaml              1/1     Running   0          3h48m   10.42.0.19   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-d4gs5   1/1     Running   0          32s     10.42.0.24   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-g2gtw   1/1     Running   0          32s     10.42.0.22   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-kf4hj   1/1     Running   0          32s     10.42.0.23   lima-rancher-desktop   <none>           <none>
~/Git/udemy/kubernetes-labs • main ❯❯
~/Git/udemy/kubernetes-labs • main ❯❯ k get deployments.apps 
NAME   READY   UP-TO-DATE   AVAILABLE   AGE
test   3/3     3            3           109s
~/Git/udemy/kubernetes-labs • main ❯❯
~/Git/udemy/kubernetes-labs • main ❯❯ k edit deployments.apps test 
Edit cancelled, no changes made.
~/Git/udemy/kubernetes-labs • main ❯❯ k describe deployments.apps test 
<REDACTED OUTPUT>
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  5m28s  deployment-controller  Scaled up replica set test-6546ccdcf9 from 0 to 3
~/Git/udemy/kubernetes-labs • main ❯❯
~/Git/udemy/kubernetes-labs • main ❯❯ k delete deployments.apps test  
deployment.apps "test" deleted
~/Git/udemy/kubernetes-labs • main ❯❯ kgp 
NAME         READY   STATUS    RESTARTS   AGE     IP           NODE                   NOMINATED NODE   READINESS GATES
httpd        1/1     Running   0          3h31m   10.42.0.21   lima-rancher-desktop   <none>           <none>
nginx-docs   1/1     Running   0          3h42m   10.42.0.20   lima-rancher-desktop   <none>           <none>
nginx-yaml   1/1     Running   0          3h57m   10.42.0.19   lima-rancher-desktop   <none>           <none>
~/Git/udemy/kubernetes-labs • main ❯❯
```

He asked the question on how would we generate the yaml to create a deployment with out creatinging it. I think the answer is to use `--dry-run` and rederct it to a file. 

now to create the yaml file to be used for IaC you run the following command: `k create deployment test --image httpd --replicas=10 --dry-run=client -o yaml > deploy.yaml` 
after we created the deploy.yaml file he had us clean it up a little bit by removing the following lines:
- under the metadata: `creationTimestamp: null`
- under spec: `strategy: {}`
- under templates:metadata: `creationTimestamp: null`
- under template:spec: `resources: {}`
- remove last line: `status {}`

Now we've deployed the file we just created and edited.
```
~/Git/udemy/kubernetes-labs • section3-deployments ❯❯ k apply -f deploy.yaml 
deployment.apps/test created
~/Git/udemy/kubernetes-labs • section3-deployments ❯❯ kgp
NAME                    READY   STATUS    RESTARTS   AGE     IP           NODE                   NOMINATED NODE   READINESS GATES
httpd                   1/1     Running   0          3h53m   10.42.0.21   lima-rancher-desktop   <none>           <none>
nginx-docs              1/1     Running   0          4h4m    10.42.0.20   lima-rancher-desktop   <none>           <none>
nginx-yaml              1/1     Running   0          4h19m   10.42.0.19   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-2rkmm   1/1     Running   0          68s     10.42.0.33   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-dc6gx   1/1     Running   0          68s     10.42.0.30   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-fdhqb   1/1     Running   0          68s     10.42.0.34   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-hbt7m   1/1     Running   0          68s     10.42.0.25   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-hrm4l   1/1     Running   0          68s     10.42.0.31   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-mqj2d   1/1     Running   0          68s     10.42.0.26   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-t8n2p   1/1     Running   0          68s     10.42.0.29   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-v9bxj   1/1     Running   0          68s     10.42.0.27   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-w24f9   1/1     Running   0          68s     10.42.0.28   lima-rancher-desktop   <none>           <none>
test-6546ccdcf9-wfmpt   1/1     Running   0          68s     10.42.0.32   lima-ranche-desktop   <none>           <none>
~/Git/udemy/kubernetes-labs • section3-deployments ❯❯
~/Git/udemy/kubernetes-labs • section3-deployments ❯❯ k get replicasets.apps test-6546ccdcf9 
NAME              DESIRED   CURRENT   READY   AGE
test-6546ccdcf9   10        10        10      3m31s
~/Git/udemy/kubernetes-labs • section3-deployments ❯❯❯

```

So the deployment controls a replicaset and the the relicaset is what controls the pods.

```
~/Git/udemy/kubernetes-labs • section3-deployments ❯❯❯ k describe replicasets.apps test-6546ccdcf9 
Name:           test-6546ccdcf9
Namespace:      default
Selector:       app=test,pod-template-hash=6546ccdcf9
Labels:         app=test
                pod-template-hash=6546ccdcf9
Annotations:    deployment.kubernetes.io/desired-replicas: 10
                deployment.kubernetes.io/max-replicas: 13
                deployment.kubernetes.io/revision: 1
Controlled By:  Deployment/test
Replicas:       10 current / 10 desired
Pods Status:    10 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=test
           pod-template-hash=6546ccdcf9
  Containers:
   httpd:
    Image:         httpd
    Port:          <none>
    Host Port:     <none>
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Events:            <none>
~/Git/udemy/kubernetes-labs • section3-deployments ❯❯❯
```

Next we went over some more settings in the `deploy.yaml and created a `deploy-recreate.yaml` and redeployed an older version with this file. Next we simulated a failing deployment to observe that the system(kumbernetes) dectects the failure in the rolling update and then backs off and won't allow the app to be deployed. we observed this with the watch command:

```
Every 1.0s: kubectl get pods                                                                                                     cory-macbookpro-m1pro: 12:12:38
                                                                                                                                                   in 0.072s (0)
NAME                   READY   STATUS             RESTARTS      AGE
httpd                  1/1     Running            0             44h
nginx-docs             1/1     Running            0             44h
nginx-yaml             1/1     Running            0             45h
test-748cb7fdc-7f2gk   0/1     CrashLoopBackOff   4 (80s ago)   3m18s
test-748cb7fdc-hpm2k   0/1     CrashLoopBackOff   4 (80s ago)   3m18s
test-d98b8ff45-46ft8   1/1     Running            0             16m
test-d98b8ff45-66vvb   1/1     Running            0             15m
test-d98b8ff45-8zjzz   1/1     Running            0             16m
test-d98b8ff45-cdvc7   1/1     Running            0             15m
test-d98b8ff45-n7gd7   1/1     Running            0             15m
test-d98b8ff45-q9rfx   1/1     Running            0             15m
test-d98b8ff45-vhqpw   1/1     Running            0             15m
test-d98b8ff45-vsd2h   1/1     Running            0             15m
test-d98b8ff45-x7j2m   1/1     Running            0             15m
```
## Section 3: Deployments - Namespaces

We're going to  clean up the deployments that are failing. This is left from the previous exercise.

```
~/Git/udemy/kubernetes-labs • section3-namespaces ❯❯ k get deployments.apps
NAME   READY   UP-TO-DATE   AVAILABLE   AGE
test   9/10    2            9           2d2h
~/Git/udemy/kubernetes-labs • section3-namespaces ❯❯ k delete deployments.apps test 
deployment.apps "test" deleted
~/Git/udemy/kubernetes-labs • section3-namespaces ❯❯ k get pods
NAME         READY   STATUS    RESTARTS   AGE
httpd        1/1     Running   0          2d5h
nginx-docs   1/1     Running   0          2d6h
nginx-yaml   1/1     Running   0          2d6h
~/Git/udemy/kubernetes-labs • section3-namespaces ❯❯ k delete pod httpd nginx-docs nginx-yaml 
pod "httpd" deleted
pod "nginx-docs" deleted
pod "nginx-yaml" deleted
~/Git/udemy/kubernetes-labs • section3-namespaces ❯❯ k get pods
No resources found in default namespace.
~/Git/udemy/kubernetes-labs • section3-namespaces ❯❯
```

The output of the last command "No resources found in default namespace." what does that mean? We can get a list of namespaces to see: `k get namespaces` A name space is just a logical grouping of resources. Here is what the docs say about it:[Kubernetes Documentation / Concepts / Overview / Objects In Kubernetes / Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) an interestiong note on the docs page. 

**Note:**
>For a production cluster, consider not using the default namespace. Instead, make other namespaces and use those.

Next we created some directories and then created a namespace in one of the newly created directories.
```
~/Git/udemy/kubernetes-labs • section3-namespaces ❯❯❯ mcd dudes-projects
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯ k create ns dudes-project --dry-run=client -o yaml > namespace.yaml
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯ k apply -f namespace.yaml
namespace/dudes-project created
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯ k get ns
NAME              STATUS   AGE
default           Active   3d8h
dudes-project     Active   7s
kube-node-lease   Active   3d8h
kube-public       Active   3d8h
kube-system       Active   3d8h
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯
```
Next we created a namespace and then created a pod and did so with the namespace.
```
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯ k run dude-mealie --image=nginx -ndudes-project 
pod/dude-mealie created
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯ kgp
No resources found in default namespace.
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯ kgp -n dudes-project
NAME          READY   STATUS    RESTARTS   AGE   IP           NODE                   NOMINATED NODE   READINESS GATES
dude-mealie   1/1     Running   0          49s   10.42.0.77   lima-rancher-desktop   <none>           <none>
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯
```

Now we are setting the namespace to the current context so we don't constantly have to use the -n flag when listing resources and what not. 
```
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯ k config current-context 
rancher-desktop
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯ k config set-context --current --namespace=dudes-project
Context "rancher-desktop" modified.
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯ kgp
NAME          READY   STATUS    RESTARTS   AGE   IP           NODE                   NOMINATED NODE   READINESS GATES
dude-mealie   1/1     Running   0          10m   10.42.0.77   lima-rancher-desktop   <none>           <none>
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯ kgp -n default
No resources found in default namespace.
~/Git/udemy/kubernetes-labs/dudes-projects • section3-namespaces ❯❯❯ 
```

## Section 3: Deployments - Our first Application 

We've now created a directory for our project and the name space. We then createed the deployment for the pod to run the application in. and we setup port forwarding to look at the app in the browser. While clicking around in the app we coulld see that the version we used was old and not the latest. so we then modified the `deployment.yaml` file to use the latest version on their github and loaded the `kubectl get pods` command in a `watch` session so we can watcht he upgrade. 
```
~/Git/udemy/kubernetes-labs/dudes-projects • section3-our-1st-app ❯ k create deployment dudes-mealie --image=nginx --dry-run=client -o yaml > deployment.yaml
~/Git/udemy/kubernetes-labs/dudes-projects • section3-our-1st-app ❯❯ vi deployment.yaml
~/Git/udemy/kubernetes-labs/dudes-projects • section3-our-1st-app ❯❯ k apply -f deployment.yaml 
deployment.apps/dudes-mealie created
~/Git/udemy/kubernetes-labs/dudes-projects • section3-our-1st-app ❯❯ kgp
NAME                            READY   STATUS              RESTARTS   AGE   IP           NODE                   NOMINATED NODE   READINESS GATES
dude-mealie                     1/1     Running             0          52m   10.42.0.77   lima-rancher-desktop   <none>           <none>
dudes-mealie-755cf89bf4-tnkcb   0/1     ContainerCreating   0          6s    <none>       lima-rancher-desktop   <none>           <none>
~/Git/udemy/kubernetes-labs/dudes-projects • section3-our-1st-app ❯❯
~/Git/udemy/kubernetes-labs/dudes-projects • section3-our-1st-app ❯❯ k delete pod dude-mealie 
pod "dude-mealie" deleted
~/Git/udemy/kubernetes-labs/dudes-projects • section3-our-1st-app ❯❯ kgp
NAME                            READY   STATUS    RESTARTS   AGE     IP           NODE                   NOMINATED NODE   READINESS GATES
dudes-mealie-755cf89bf4-tnkcb   1/1     Running   0          4m26s   10.42.0.78   lima-rancher-desktop   <none>           <none>
~/Git/udemy/kubernetes-labs/dudes-projects • section3-our-1st-app ❯❯ k port-forward pods/dudes-mealie-755cf89bf4-tnkcb 9000 
Forwarding from 127.0.0.1:9000 -> 9000
Forwarding from [::1]:9000 -> 9000
Handling connection for 9000
Handling connection for 9000
...
~/Git/udemy/kubernetes-labs/dudes-projects • section3-our-1st-app ❯❯ vi deployment.yaml 
~/Git/udemy/kubernetes-labs/dudes-projects • section3-our-1st-app ❯❯ watch -n1 kubectl get pods
Every 1.0s: kubectl get pods                                                                                                       cory-macbookpro-m1pro: 00:01:44
                                                                                                                                                     in 0.100s (0)
NAME                            READY   STATUS              RESTARTS   AGE
dudes-mealie-755cf89bf4-tnkcb   1/1     Running             0          22m
dudes-mealie-7b46ff9b4f-v48hv   0/1     ContainerCreating   0          3s
```

After we did the upgrade of the application we connected to it again with port forwarding and this new version I set to latest which was widely different than the one in the tutorial. Anyways it worked and was pretty cool watching it uptrade by keeping the onld version online while it downloaded the latest image.  Here is the releases page for the application we setup: [mealie-recipes/mealie releases](https://github.com/mealie-recipes/mealie/releases)

## Section 4. Networking

In this section the first video is sort of Networking 101 and refresher but then talks about the CNI that Kubernetes uses. We then cehck out what CNI that the RancherDesktop is using by shelling into the VM that runs Kubernetes and looking at it's config. 

```
~/Git/udemy/kubernetes-labs/dudes-projects • section4-networking-intro ❯ rdctl shell bash
/Users/cory/Git/udemy/kubernetes-labs/dudes-projects $ tree /etc/cni/
/etc/cni/
└── net.d
    └── 10-flannel.conflist

1 directories, 1 files
/Users/cory/Git/udemy/kubernetes-labs/dudes-projects $
/Users/cory/Git/udemy/kubernetes-labs/dudes-projects $ cat /etc/cni/net.d/10-flannel.conflist 
{
  "name":"cbr0",
  "cniVersion":"0.3.1",
  "plugins":[
    {
      "type":"flannel",
      "delegate":{
        "hairpinMode":true,
        "forceAddress":true,
        "isDefaultGateway":true
      }
    },
    {
      "type":"portmap",
      "capabilities":{
        "portMappings":true
      }
    }
  ]
}
/Users/cory/Git/udemy/kubernetes-labs/dudes-projects $
```

Note: very cool that you can see this info from RancherDesktop. `rdctl -h` for more info about ranchDesktop from the cli.

Next section is talking about services. So basically we ended up creating a service that exposes our mealie app on port 9000 and then we set the type to "LoadBalancer" to be able to access it via localhost or it's external IP. this is much more useful than having to setup protforwding... to say the least. 

```
~/Git/udemy/kubernetes-labs/deployments • section4-networking-intro ❯❯❯ k get svc
NAME           TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
dudes-mealie   ClusterIP   10.43.120.47   <none>        9000/TCP   45s
~/Git/udemy/kubernetes-labs/deployments • section4-networking-intro ❯❯❯
~/Git/udemy/kubernetes-labs/deployments • section4-networking-intro ❯❯❯ k get svc dudes-mealie -o yaml > service.yaml
~/Git/udemy/kubernetes-labs/deployments • section4-networking-intro ❯❯❯ vi service.yaml
~/Git/udemy/kubernetes-labs/deployments • section4-networking-intro ❯❯❯ k delete svc dudes-mealie
service "dudes-mealie" deleted
~/Git/udemy/kubernetes-labs/deployments • section4-networking-intro ❯❯❯ k get svc
No resources found in dudes-project namespace.
~/Git/udemy/kubernetes-labs/deployments • section4-networking-intro ❯❯❯ k apply -f service.yaml
service/dudes-mealie created
~/Git/udemy/kubernetes-labs/deployments • section4-networking-intro ❯❯❯ k get svc
NAME           TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)          AGE
dudes-mealie   LoadBalancer   10.43.217.50   192.168.5.15   9000:30462/TCP   3s
~/Git/udemy/kubernetes-labs/deployments • section4-networking-intro ❯❯❯
```
The next section he discusses the Ingress controller for Kubernetes. This is a more advanced topic but basically the thing to know is that it is used with DNS and URL's. 




