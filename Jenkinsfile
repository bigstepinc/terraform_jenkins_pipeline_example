pipeline {
    agent any
    
    environment {
        SKIP_MANUAL_APPROVAL_STAGE = "true"
        
        TERRAFORM_CMD = "/usr/local/bin/terraform"

        METALCLOUD_API_KEY = credentials("METALCLOUD_API_KEY")
        METALCLOUD_USER_EMAIL = credentials("METALCLOUD_USER_EMAIL")
        METALCLOUD_ENDPOINT = credentials("METALCLOUD_ENDPOINT")
        TF_VAR_datacenter = credentials("TF_VAR_datacenter")
    }

   stages { 
    stage("Run terraform init"){
        steps{
            sh "'$TERRAFORM_CMD' init -input=false"
        }
    }

    stage("Run terraform plan"){
      steps{
        sh "'$TERRAFORM_CMD' plan -out='$JOB_NAME' -input=false"
      }
    }

    stage("Terraform apply approval") {
        when { expression { "$SKIP_MANUAL_APPROVAL_STAGE" == "false" } }
        steps {
            script {
                def userInput = input(id: "confirm", message: "Apply Terraform?", parameters: [ [$class: "BooleanParameterDefinition", defaultValue: false, description: "Apply terraform", name: "confirm"] ])
            }
        }
    }

    stage("Run terraform apply"){
      steps{
        sh "'$TERRAFORM_CMD' apply '$JOB_NAME'"
      }
    }   

    stage("Terraform destroy approval") {
        when { expression { "$SKIP_MANUAL_APPROVAL_STAGE" == "false" } }
        steps {
            script {
                def userInput = input(id: "confirm", message: "Run terraform destroy?", parameters: [ [$class: "BooleanParameterDefinition", defaultValue: false, description: "Destroy terraform", name: "confirm"] ])
            }
        }
    }

    stage("Run terraform destroy") {
        steps {
            sh "'$TERRAFORM_CMD' destroy -force "
        }
    }

    stage("Clean up workspace") {
        steps {
            cleanWs()
        }
    }

}

}
