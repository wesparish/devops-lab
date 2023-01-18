# Create Git repo
1. Browse to github.com and login
2. From the top-right, click your user icon, then select `Your repositories`
3. Click the green `New` button to create a new repository
4. Name the repository `devops-lab-<username>` and click the
   `Create repository` button at the bottom
5. Clone the new repository locally
   1. Add an SSH key to your Github account if you haven't already
      * Run `ssh-keygen` and hit enter until it completes
      * Run `cat ~/.ssh/id_rsa.pub` to print your **public** SSH key
      * Browse to your Github settings page and select `SSH and GPG keys` from
        the left navbar
      * Click the green `New SSH key` button and paste in your **public** SSH
        key printed earlier
   2. Clone the newly created `devops-lab-<username>` Git repo
      * Browse to your new Git repo on github.com (top-right,
        `Your repositories`, `devops-lab-<username>`)
      * Copy the SSH clone URL, e.g.
        `git@github.com:<username>/devops-lab-<username>.git`
      * From your workstation, run `git clone <SSH clone URL>` to clone the
        repo locally
   3. Create placeholder subdirectories to be used later
      ```
      for dir in docker-images/nginx-hello-world/ helm-charts/nginx-demo/ ansible-playbooks/helm-deployment-playbook/ ; do
        mkdir -p $dir && touch $dir/.placeholder
      done
      ```
   4. Add / commit / push the changes to Github
      ```
      git status
      git add .
      git status
      git commit -m "Initial creation of directory structure"
      git push origin main
      git status
      ```
   5. Refresh your repository on Github to ensure your new subdirectories show
      up

**Note**: Remember, aside from `fetch`, `push` and `pull`, all Git commands run locally. To interact with Github, you must run push/pull commands.