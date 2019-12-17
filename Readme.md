Bigstep Metalcloud Terraform Jenkins pipeline examples
==================
This repo contains examples on how to execute terraform via jenkins pipelines


Requirements
------------
-	Bigstep Metalcloud Terraform Provider already installed https://github.com/bigstepinc/terraform-provider-metalcloud
- An up & running Jenkins 
- A Bigstep Metalcloud account

Setup
------------
To get started do the following:
 - Add global credentials in Jenkins for Metalcloud variables: METALCLOUD_API_KEY, METALCLOUD_USER_EMAIL, METALCLOUD_ENDPOINT and TF_VAR_datacenter
  ![Alt text](/images/jenkins-global-credentials.png?raw=true "")
 - Create a new pipeline in Jenkins
 - Select 'This project is parameterized' check-box and add 2 new parameters (optional step)
    ![Alt text](/images/pipeline-parameters.png?raw=true "")
 - Setup the job to pull the repo for the Jenkinsfile (pipeline). 
   ![Alt text](/images/pipeline-script-from-scm.png?raw=true "")
