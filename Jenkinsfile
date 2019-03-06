pipeline {
  environment {
   //login id/docker reposotory defined in Jenkins followed by registry name created in dockerhub
    registry = "jimish22/jenkin-docker"
    //credential = Id given jenkins
    registryCredential = 'DockerHub'
    dockerImage = ''
    //checked if any container is running with same name node-app container Id will store same & stage cleanup will close that container
    containerId = sh(script: 'docker ps -aqf "name=node-app"', returnStdout: true)
  }
  agent any
  tools {nodejs "node" }
  stages {
 //   stage('Cloning Git') {
     // steps {
       // git 'https://github.com/dipankar2019/node-todo-frontend'
     // }
    //}
    stage('Build') {
       steps {
         sh 'npm install'
       }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Building image') {
      steps{
        script {
          //will pisck registry from variable defined
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
     }
}
