pipeline {
  agent { label 'jenkins-agent'} 

  environment {
    APP_NAME = "register-app-pipeline"
  }
  stages {
    stage('cleanup the workplace') {
      steps {
        cleanWs()
      }
    }
    
    stage('checkout the scm') {
      steps {
          git branch: 'main', url: 'https://github.com/Anusuya-murugiah/gitops-register-app.git', credentialsId: 'github-token'  
      }
    }

    stage('update the deployment Tags') {
      steps {
        sh """
          cat deployment.yaml
          sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
        """
      }
    }

    stage('push the changed deployment file to git') {
      steps {
        sh """
          git config --global user.name "Anusuya-murugiah"
          git config --global user.email "anusuya211998@gmail.com" 
          git add deployment.yaml
          git commit -m "updated the deployment file"
        """
        withCredentials([gitUsernamePassword(credentialsId: 'github-token', gitToolName: 'Default')]) {
           sh "git push https://github.com/Anusuya-murugiah/gitops-register-app main"
        }
      }
    }
    
  }
}
