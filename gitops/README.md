# 🛠 GitOps

## What is GitOps?
It's like version control for your Kubernetes cluster. You have your Infrasturture as Code (IaC) in form of Terraform files or K8s manifest files which will set up the required server for you to run your app cluster and to manage changes made to these files we use GitOps practices. 

![image](https://user-images.githubusercontent.com/55504616/224292786-e1acc4c5-5e64-4358-89c4-6d4b0de25e82.png)

## Benefits
You can keep all your IaC files in a single git repository from where all the team members can centrally have access to it and make changes which will undergo the CI pipeline processes. After the changes get approved and merged it will undergo the CD pipeline to deploy the latest changes to the K8s cluster so that everything is up to date.

This proves that **Git is the single piece of truth**. Once deployment starts it's either push or pull deployment. **Push deployment** is done by CI/CD pipeline services like Jenkins and GitLab which will push the changes to the K8s cluster. Whereas in **Pull deployment** an agent is already present inside the cluster that will monitor your git repo and keep the cluster in sync with the repo. It regularly monitors it for any changes being made or not. Some famous examples are **ArgoCD** or **fluxCD**. 
