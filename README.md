
# About
This lab is indended to be a challenge to implement the following technologies:
   - Ansible
   - Helm
   - Git
   - Nexus
   - K8s

# Summary
Using Ansible, deploy an example Helm chart from a private nexus server that
runs a single nginx web server pod with a NodePort to K8s. The entire
solution should be committed and pushed to a git repository on Github and
the Helm chart should be stored in a private Nexus server.

# Steps
## 1 Create Git repo
Create a new git repo name `devops-lab-<username>` to store your solution in.
The solution should be organized in this directory so it easily understable by
others. This is important in the industry as multiple team members will likely
be working with code that you write.

Recommended directory layout:
```
devops-lab-<username>/
   helm-charts/
      nginx-demo/
   ansible-playbooks/
      helm-deployment-playbook/
```

Spoiler: [Create Git Repo](solutions/create_git_repo.md)

## 2 Create Nexus Helm repo
Spoiler: [Create Git Repo](solutions/create_nexus_repo.md)

## 3 Create Helm chart
Spoiler: [Create Git Repo](solutions/create_helm_chart.md)

## 4 Create Ansible playbook
Spoiler: [Create Git Repo](solutions/create_ansible_playbook.md)

## 5 Deploy Helm chart with Ansible
Spoiler: [Create Git Repo](solutions/deploy_helm_chart_with_ansible.md)