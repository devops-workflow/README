
# How to install Jenkins as a docker container and setup framework


Setup scripts are in the scripts repository

Run:
- docker-install.sh
- docker-jenkins-install.sh
- slave-setup.sh

Then start setting up Jenkins
- Setup master-host node
- Install plugins
  - Automate with SaltStack ?
  - Was done with Puppet before
- Needed config before bootstrap
  - Credential setup for Jenkins_Job_Builder
  - Implied label: All
  - Git name and email
  - /bin/sh might not be bash. If so fix on system or in Jenkins config
- Bootstrap DevOps Workflow framework
  - TODO: write script to import these
  - Copy jobs
    - Jenkins_Config (need to create. Will handle some of above issues)
    - Jenkins_Job_Builder
    - Jenkins_Tools
    - Tool-Python-Setup-Nodes
