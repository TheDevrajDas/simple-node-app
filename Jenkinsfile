pipeline {
  environment {
    registry = "darshhd/simple-node-app"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/darshhd/simple-node-app.git'
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
        sh "docker run -d -p 8000:8000 ${dockerImage.imageName()}"
      }
    }
//    stage('Running the app') {
//      steps{
//        sh "node app.js"
//      }
//    }
  }
}
