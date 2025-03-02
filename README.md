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

