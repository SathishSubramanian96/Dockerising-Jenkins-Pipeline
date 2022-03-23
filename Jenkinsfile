pipeline {
  environment {
    registry = "sathishsubramanian/dockerising_jenkins_piepeline"
    registryCredential = 'dockerhub'
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
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
 
  }
}

node {
    stage('Execute Image'){
        def customImage = docker.build("sathishsubramanian/dockerising_jenkins_pipeline:${env.BUILD_NUMBER}")
        customImage.inside {
            sh 'echo Hello'
        }
    }
}
