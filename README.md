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
test-6546ccdcf9-wfmpt   1/1     Running   0          68s     10.42.0.32   lima-rancher-desktop   <none>           <none>
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

