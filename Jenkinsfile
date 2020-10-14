pipeline {
  parameters {
      booleanParam(name: 'Refresh',
                  defaultValue: true,
                  description: 'Read Jenkinsfile and exit.')
  }
  
  
  environment {
    registry = "192.168.1.152:5000/k4mrul/myweb"
    dockerImage = ""
  }

  agent any

  stages {
    
    stage('Read Jenkinsfile') {
        when {
            expression { return params.Refresh == true }
        }
        steps {
          echo("stop")
        }
    }

    stage('Checkout Source') {
      steps {
        git 'https://github.com/k4mrul/playjenkins.git'
      }
    }

    stage('Build imageeee') {
      steps{
        script {
          dockerImage = docker.build registry + ":latest"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
