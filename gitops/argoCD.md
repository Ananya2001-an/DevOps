# 🐙 ArgoCD
ArgoCD is one of the most famous GitOps CD tools. 

## Why we need it?
Now CI/CD pipelines like Jenkins can perform CD operations but there are some issues. 

Let's say we made some chages to our app's code and Jenkins wil start its operations in this order:
1. Test changes
2. Build new image 
3. Push to Docker repo
4. Also update image tag/version in K8s manifest files (Deployments)

![image](https://user-images.githubusercontent.com/55504616/224306767-4e15fdfb-1ac3-4b5d-9f16-2de4f3d9b295.png)

Above steps were part of CI pipeline and later it will start with the CD processes where it will deploy the changes to the K8s cluster but like we said before there are some issues.
1. We need to install kubectl on Jenkins to run kubectl commands
2. Configure K8s credentials for accessing the cluster
3. Also if cluster is running on a cloud provider like AWS then we would have to configure credentials for it as well 
4. Huge security issue
5. And most important issue is that Jenkins doesn't have any visibility of the deployment status (what's happening inside the cluster)

So now comes ArgoCD to the rescue. It's already present inside your cluster and monitors the changes happening inside the gitops repo where you have all the manifests. Does automatic syncing and thus removes issues of security or need of credential access. Moreover it gives updates on the cluster status whether the desired state is met or not. 

So as soon as Jenkins updates the K8s manifest file in separate gitops repo (generally we have our IaC files in a separate repo than the app's code to reduce CI complexity), ArgoCD will detect the changes and sync our cluster with it. This leads to separation of concerns. (Automated CI/CD pipeline)

![image](https://user-images.githubusercontent.com/55504616/224305049-b9618741-9d85-4488-96d4-2474a8c007b6.png)

## Benefits
If somonone manually tries to make any changes to the cluster ArgoCD will revert them since the git repo (desired state) and cluster(actula state) don't match. This means that Git is the single source of truth. Also if any changes deployed break the cluster, it will automatically rollback to the previous state.


**ArgoCD is an extension of K8s**. It uses K8s functionalities like etcd to store data and controller to manage desired and actual state of cluster.

We can also use ArgoCD with multiple clusters. We will manage just 1 argoCD instance which will make changes to all K8s clusters.  

## How to configure it?
We install argoCD inside the cluster, and in the git repo we craete an **application.yml** file which has the config for ArgoCD. It has options for syncing and automatic healing and the 2 most important things that are given are:
1. Git repo to monitor
2. Cluster to apply changes to

Sample:

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: myapp-argo-application
  namespace: argocd
spec:
  project: default

  source:
    repoURL: https://gitlab.com/nanuchi/argocd-app-config.git
    targetRevision: HEAD
    path: dev
  destination: 
    # connects to the kubernetes internal service for apiserver 
    server: https://kubernetes.default.svc
    # a new namespace will be defined to create the given components in dev folder/path
    namespace: myapp

  # by default sync options are disabled for security mechanism
  syncPolicy:
    syncOptions:
    # argocd creates namespace in cluster if not found (myapp)
    - CreateNamespace=true

    # does automatic syncing every 3 mins if any changes to components config file made in the source gitops repo
    automated:
      selfHeal: true
      prune: true

```
