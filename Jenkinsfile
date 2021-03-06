pipeline {
    agent any
    
    environment {
        METALCLOUD_API_KEY = credentials("METALCLOUD_API_KEY") // the METALCLOUD_API_KEY credentials defined in Jenkins 
        METALCLOUD_USER_EMAIL = credentials("METALCLOUD_USER_EMAIL") // the METALCLOUD_USER_EMAIL credentials defined in Jenkins 
        TF_VAR_datacenter = credentials("TF_VAR_datacenter")// the TF_VAR_datacenter credentials defined in Jenkins
        TF_VAR_endpoint = credentials("TF_VAR_endpoint")
    }

    parameters {
        booleanParam(name: 'SKIP_MANUAL_APPROVAL_STAGE', defaultValue: true, 
            description: 'if SKIP_MANUAL_APPROVAL_STAGE is true, no manual approval is needed before running terraform apply, destroy') 
        string(name: 'TERRAFORM_PATH', defaultValue: "terraform", 
            description: 'Absolute path to terraform binary') 
    }

   stages { 
    stage("Run terraform init"){
        steps{
            sh "'${params.TERRAFORM_PATH}' init -input=false"
        }
    }

    stage("Run terraform plan"){
      steps{
        sh "'${params.TERRAFORM_PATH}' plan -out='$JOB_NAME' -input=false"
      }
    }

    stage("Terraform apply approval") {
        when { expression { params.SKIP_MANUAL_APPROVAL_STAGE != true } }
        steps {
            script {
                def userInput = input(id: "confirm", message: "Apply Terraform?", parameters: [ [$class: "BooleanParameterDefinition", defaultValue: false, description: "Apply terraform", name: "confirm"] ])
            }
        }
    }

    stage("Run terraform apply"){
      steps{
        sh "'${params.TERRAFORM_PATH}' apply '$JOB_NAME'"
      }
    }   

    stage("Terraform destroy approval") {
        when { expression { params.SKIP_MANUAL_APPROVAL_STAGE != true } }
        steps {
            script {
                def userInput = input(id: "confirm", message: "Run terraform destroy?", parameters: [ [$class: "BooleanParameterDefinition", defaultValue: false, description: "Destroy terraform", name: "confirm"] ])
            }
        }
    }

    stage("Run terraform destroy") {
        steps {
            sh "'${params.TERRAFORM_PATH}' destroy -force "
        }
    }

}
post {
    always {
        cleanWs()
    }
}

}
