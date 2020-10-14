pipeline {

  //environment {
  //  registry = "192.168.1.152:5000/k4mrul/myweb"
  //  dockerImage = ""
  //}

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/k4mrul/playjenkins.git', branch:'master'
      }
    }

    stage('Build image') {
      steps{
        script {
          
          //dockerImage = docker.build registry + ":$BUILD_NUMBER"
          dockerImage = docker.build("192.168.1.152:5000/k4mrul/mytestweb:${env.BUILD_ID}")
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          //docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') { }
          docker.withRegistry( "" ) {
            dockerImage.push("latest")
            dockerImage.push("${env.BUILD_ID}")
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
