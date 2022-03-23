pipeline {
  environment {
    registry = "sathishsubramanian/dockerising_jenkins_piepeline"
    registryCredential = 'dockerHub'
  }
  agent any
  stages {
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
     steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Remove Image') {
      steps{
        bat "docker rmi $registry:$BUILD_NUMBER"
      }
    }
 
  }
}

node {
    stage('Execute Image'){
        def customImage = docker.build("sathishsubramanian/dockerising_jenkins_piepeline:${env.BUILD_NUMBER}")
        customImage.inside {
            bat 'echo Hello'
        }
    }
}
