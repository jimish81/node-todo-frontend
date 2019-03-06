pipeline {
  environment {
    //login id/docker reposotory defined in Jenkins
    registry = "dipankar435/docker-repo"
    //credential = Id given jenkins
    registryCredential = 'dockerhub'
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
    stage('Push Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Cleanup') {
      when {
                not { environment ignoreCase: true, name: 'containerId', value: '' }
        }
      steps {
        sh 'docker stop ${containerId}'
        sh 'docker rm ${containerId}'
      }
    }
    stage('Run Container') {
      steps {
        sh 'docker run --name=node-app -d -p 3000:3000 $registry:$BUILD_NUMBER &'
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
