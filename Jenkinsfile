pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    IMAGE_NAME = 'sidoreldocker/jenkins-docker-hub'
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t $IMAGE_NAME .'
      }
    }
    stage('Login') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'sidoreldocker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
        }
      }
    }
    stage('Push') {
      steps {
        sh 'docker push $IMAGE_NAME'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
