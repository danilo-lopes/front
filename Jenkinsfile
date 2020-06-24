pipeline {

  environment {
    registry = "dansolo7/front"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/danilo-lopes/front'
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
          withDockerRegistry([ credentialsId: 'dockerhub_credential', url: "" ]) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "front-k8s.yaml", kubeconfigId: "kubernetes_configfile")
        }
      }
    }

  }
}
