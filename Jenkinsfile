
pipeline {
  agent { 
  label 'ansiblenode'
  }
  environment {
   VASANTH=credentials('ec2-private-key') 
  }
  
  stages {
    
    
    stage('CheckOutCode'){
      steps {
        sh "whoami"
        git 'https://github.com/vasanthdevops-program/jekins-ansible-dynimc-inv.git' 
      }
    }   
     
    //Execute Terraform Script
    stage('Execute Terraform Script'){
      steps {
       sh "terraform -v"
       sh "terraform  -chdir=terraformscripts init"
       sh "terraform  -chdir=terraformscripts apply --auto-approve"
      }
    }
    
    
    stage('RunPlaybook') {
      steps {
        sh "ansible-inventory --graph -i inventory/aws_ec2.yaml"
        sh "ansible-playbook -i inventory/aws_ec2.yaml --private-key=$VASANTH playbooks/installTomcat9.yaml --ssh-common-args='-o StrictHostKeyChecking=no' -l tomcatservers"
      }
    }
  
  }//stages closing
}//pipeline closing
