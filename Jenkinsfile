pipeline {
  agent any
//  {
//    kubernetes {
//     label 'jenkinskube'
//    }
//  }
  tools {
    maven 'maven'
    }
  environment{
    DOCKER_HOME = tool name: 'docker', type: 'dockerTool'
    VERSION = "${env.BUILD_ID}-${env.GIT_COMMIT}"
    }
//   options {
//     gitLabConnection('Gitlab')
//   }
  stages {
    stage('Check out') {
      steps {
        checkout scm
      }
    }

    stage('Build'){
        steps{
            sh "mvn clean package"
        }
    }

    stage('Docker Build'){
        steps{
            echo "Running ${VERSION} on ${env.JENKINS_URL}"
            withDockerRegistry(credentialsId: 'aksregistrykey', url: 'aksregistryuseast.azurecr.io') {
            sh 'docker build -t aksregistryuseast.azurecr.io/gs-spring-boot-docker:${BUILD_NUMBER} .'
            }
    }
   }
    stage('Docker Push') {
        steps{
            withDockerRegistry(credentialsId: 'aksregistrykey', url: 'aksregistryuseast.azurecr.io') {
              sh "docker push aksregistryuseast.azurecr.io/gs-spring-boot-docker:${BUILD_NUMBER}"
            }
        }
    }
   }
 }//tes