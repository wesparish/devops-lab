# Create Nexus Helm and Docker repos
1. Browse to nexus server web UI
2. Click `Sign In` in the top-right corner and enter administrator credentials
3. Click the cog in the top-center navbar to browse to the admin screen
4. Click `Repository -> Repositories` on the left navbar
5. Click `+ Create repository` to create a new repository
6. Create the following repositories:
   1. Helm (hosted)
      * Name: `helm-<username>`
      * Scroll down and click `Create repository` button at the bottom
   2. Docker (hosted)
      * Name: `docker-<username>`
      * Scroll down and click `Create repository` button at the bottom
7. Add helm repo locally
```
$ HELM_USER=changeme
$ HELM_PASSWORD=changeme
$ HELM_REPO_URL=http://nexus-url.com/changeme/
$ helm repo add helm-$USER ${HELM_REPO_URL} --username $HELM_USER --password $HELM_PASSWORD
$ helm repo update
```