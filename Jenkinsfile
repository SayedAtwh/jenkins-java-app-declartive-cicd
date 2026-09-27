pipeline{
  agent {
    label 'agent-1'
  }
  tools {
  jdk 'jdk-11'
  maven 'maven'
  }

  environment {
  IMAGE_NAME = "java-app-declartive"
  IMAGE_TAG = "sayedatwhdevops/"
  IMAGE_VERSION = "${BUILD_NUMBER}"
  }
  stages {
    stage(" Checkout SCM ") {
      steps {
        checkout scm
      }
    }
    stage(" build java application ") {
      steps {
        sh " mvn package install -DskipTests=true"
      }
    }
    stage(" test java application ") {
      steps {
        sh " mvn test"
      }
    }
    stage(" Build docker image ") {
      steps {
        sh " docker build -t ${IMAGE_NAME}:${IMAGE_VERSION} ."
      }
    }
    stage(" Docker login into dockerHub "){
      steps {
       withCredentials([string(credentialsId: 'DOCKER_USERNAME', variable: 'DOCKER_USERNAME'), string(credentialsId: 'DOCKER_PASSWORD', variable: 'DOCKER_PASSWORD')]) 
      }
    }
    stage(" Push docker image ") {
      steps {
        sh " docker tag ${IMAGE_NAME}:${IMAGE_VERSION} ${IMAGE_TAG}:${IMAGE_NAME}:${IMAGE_VERSION} "
        sh " docker push ${IMAGE_TAG}:${IMAGE_NAME}:${IMAGE_VERSION} " 
      }
    }

  }
}