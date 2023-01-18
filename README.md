
# About
This lab is indended to be a challenge to implement the following technologies:
   - Ansible
   - Helm
   - Git
   - Nexus
   - K8s
   - Docker

# Summary
Using Ansible, deploy an example Helm chart from a private nexus server that
runs a single nginx web server pod with a NodePort to the K8s namespace
`nginx-demo`. The entire solution should be committed and pushed to a git
repository on Github and the Helm chart should be stored in a private Nexus
server. Once running, upgrade your deployed Nginx instance to a custom Docker
image to override the default HTML start page with a custom "Hello World" page.

**Note: The `solutions` subdirectory has examples of commands I used to
accomplish these goals. You should attempt to do this yourself before
looking at the solutions.**

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
   docker-images/
      nginx-hello-world/
```

Ensure you commit/push all changes to Github.

Spoiler: [Create Git Repo](solutions/create_git_repo.md)

## 2 Create Helm and Docker Nexus repositories
Using the Nexus web UI, create a new Helm3 repository. The Helm chart created
in this lab will be pushed to this new Helm3 Nexus repo.

Ansible will also deploy the Helm chart from Nexus (rather than the local
filesystem).

Also create a Docker repository for use later in the lab.

Spoiler: [Create Nexus Helm and Docker repos](solutions/create_nexus_repo.md)

## 3 Create Helm chart
Create the Helm chart (Helm3) locally in the git repo from step 1.

Ensure you test the new chart using the `helm` CLI to deploy to K8s. Delete any
chart deployments when you are done testing. The chart should deploy to the
namespace `nginx-demo`.

Don't forget to publish your newly created Helm chart to Nexus for consumption
by Ansible.

Notes:
* It is acceptable to use the community nginx Docker image
* Recommend naming the chart `nginx-demo`
* Recommend using the `helm create` CLI (see `helm create --help`)

Spoiler: [Create Helm chart](solutions/create_helm_chart.md)

## 4 Create Ansible playbook
Create an ansible playbook to perform the deployment of the new Helm chart.

Notes:
* Helm chart should be installed from a Nexus URL, not a local subdirectory
* Recommend using the kubernetes.core.helm module (https://docs.ansible.com/ansible/latest/collections/kubernetes/core/helm_module.html)
* Recommend allowing the kubernetes.core.helm module to use `~/.kube/config
credentials for access`
* No values.yaml overrides are necessary for this lab
* Recommend naming ansible-playbook `helm-deployment-playbook`

Spoiler: [Create Ansible playbook](solutions/create_ansible_playbook.md)

## 5 Deploy Helm chart with Ansible
Demonstrate the deployment of the new Chart from Nexus with Ansible to K8s.

Using a web browser, browse to the IP of any K8s cluster members on the
nodePort created by K8s. You should see the Nginx welcome page.

### Extra credit
Demonstrate the deletion of the new Helm release (deployed instance of a chart)
with Ansible by parameterizing the `release_state` attribute of the
kubernetes.core.helm module. Add an Ansible arg to select either `present` or
`absent` to control whether the Helm chart should be deployed or not.

Spoiler: [Deploy Helm chart with Ansible](solutions/deploy_helm_chart_with_ansible.md)

## 6 Create a custom Nginx Docker image
Create a custom Docker image (Dockerfile + static HTML content file),
controlled in your Git repo, that is `FROM nginx:latest` and overrides the
default HTML page that ships with Nginx to be a custom "Hello World" page.

Publish this new Docker image to your Nexus server in a Docker repo.

Notes:
  * Recommend storing the Docker image code in
    `docker-images/nginx-hello-world/`

Spoiler: [Create a custom Nginx Docker image](solutions/create_custom_nginx_docker_image.md)

## 7 Devops Workflow
Update your Helm chart to pin this new custom `nginx-hello-world` instead of
`nginx:latest`, and publish to youre Nexus Helm repo.

Using your Ansible playbook, update your Helm release with the new Helm chart
changes to upgrade your Nginx deployment to the new custom Docker image.