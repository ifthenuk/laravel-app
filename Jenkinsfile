pipeline {
  agent { label 'belajar-jenkins' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    IMAGE_NAME = 'ifthenuk/laravel10-jenkins'
    IMAGE_TAG = 'latest'
    APP_NAME = 'jenkins-example-laravel'
  }
  stages {
    stage('Docker Build') {
      steps {
        sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
      }
    }
    stage("Docker push") {
        environment {
            DOCKER_USERNAME = credentials("docker-user")
            DOCKER_PASSWORD = credentials("docker-password")
        }
        steps {
            sh "docker login --username ${DOCKER_USERNAME} --password ${DOCKER_PASSWORD}"
            sh "docker tag ${IMAGE_TAG} ${IMAGE_NAME}"
            sh "docker push ${IMAGE_NAME}"
        }
    }
    stage("Deploy to staging") {
        steps {
            sh "docker run -d --rm -p 8081:80 --name laravel-jenkins ${IMAGE_NAME}:${IMAGE_TAG}"
        }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}