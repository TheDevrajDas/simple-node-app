pipeline {
  environment {
    registry = "dockerdevrajdas/simple-node-app"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/TheDevrajDas/simple-node-app.git'
      }
    }
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
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
    stage('Pulling from dockerhub') {
      steps{
        sh "docker pull $registry:$BUILD_NUMBER"
      }
    }
//    stage('Running the app') {
//      steps{
//        sh "node app.js"
//      }
//    }
  }
}
