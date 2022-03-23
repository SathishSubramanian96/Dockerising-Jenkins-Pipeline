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
      script { 
        docker container run --detach --publish 8080:80 --name container_new
      }
        customImage.inside("--entrypoint=''") {
            bat "echo Hello"        }
    }
}
