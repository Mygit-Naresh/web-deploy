pipeline {
 agent {
    node {
        label 'jenkins-agent-2'
    }
 }
//  environment {
//       versioncheck = ''
//       nexus_url = '172.31.16.69:8081'
//  }

   parameters {
//         // booleanParam, choice, file, text, password, run, or string
        booleanParam(defaultValue: '', description: '', name: 'deploy')
        booleanParam(defaultValue: '', description: '', name: 'destroy')
      //   string(defaultValue: '', description: 'What is version?', name: "version")
      //   string(defaultValue: '', description: 'What is environment?', name: "environment")
      //   string(defaultValue: 'Write deploy for app deploy OR write destroy for app destroy', description: 'What is application?', name: "application")
//         text(defaultValue: "This is a multiline\n text", description: "Multiline Text", name: "textExample")
//         choice(choices: 'US-EAST-1\nUS-WEST-2', description: 'What AWS region?', name: 'choiceExample')
//         password(defaultValue: "Password", description: "Password Parameter", name: "passwordExample")
   }
//  parameters {
//       choice(choices: 'PROD\nDEV', description: 'Choose PROD or DEV?', name: 'choice')
//  }
   
   options {
                timeout(time: 1, unit: 'HOURS')
                ansiColor('xtrem')
           }
    

    stages {
   //   stage('checkout_from_scm') {
   //   steps {
   //       git branch: 'main', changelog: false, poll: false, url: 'https://github.com/Mygit-Naresh/catalogue.git'
   //     }
   //  }
   //  stage('print version and enviroment') {
   //    steps {
   //     script {
   //      echo  params.version
   //      echo  params.environment
   //     }

   //  }
   //  }
       stage('terraform init from tfvars') {
      steps {
      
        sh """
          cd terraform
          terraform init  --backend-config=${params.environment}/backend.tf -reconfigure
          

        """
       

    }
    }
    
   // stage('terraform plan from tfvars') {
   //    steps {
   //     script {
   //      sh """
   //        cd terraform
   //        terraform plan -var-file=${params.environment}/${params.environment}.tfvars -var="appversion=${params.version}"
   //        """
   //     }

   //  }
   //  }
     stage('terraform apply from tfvars') {
           when {
            expression {
               params.deploy
            }
         }
      steps {
       
         script {
      
        sh """
          cd terraform
          terraform apply -auto-approve -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}" 
          """
       }

    }
    }
         stage('terraform destroy from tfvars') {
            when {
            expression {
               params.destroy
            }
         }
      steps {
           
       script {
       
        sh """
          cd terraform
          terraform destroy -auto-approve -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}" 
          """
       }

    }
    }

    }
      post {
   always {
      echo "Check you status below failure or success"
      
   }
    failure {
        echo "your build failed"
    }
    success {
        echo "your build is success thumbs up"
        
    }
  }
}
