pipeline {

  environment {
    //registry = "192.168.1.81:5000/justme/myweb"
    registry = "ahmedcheibani/myweb"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/Ahmed-Cheibani/playjenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', 'dockerhub_login' ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Kubernetes Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kube_config")
        }
      }
    }

  }

}
