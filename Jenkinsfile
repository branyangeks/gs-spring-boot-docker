pipeline {
  agent any
  tools {
    maven 'maven'
    }
  environment{
    DOCKER_HOME = tool name: 'docker', type: 'dockerTool'
    VERSION = "${env.BUILD_ID}-${env.GIT_COMMIT}"
    }
  stages {
    stage('Check out') {
      steps {
        checkout scm
      }
    }

    stage('Docker Build and Push'){
        steps{
            echo "Running ${VERSION} on ${env.JENKINS_URL}"
            withCredentials([usernamePassword(credentialsId: 'aksregistrykey', passwordVariable: 'ACR_PASSWORD', usernameVariable: 'ACR_ID')]) {
              sh 'docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}'
              sh 'mvn compile jib:build'
            //  sh 'docker build -t aksregistryuseast.azurecr.io/gs-spring-boot-docker:${BUILD_NUMBER} .'
            }        
    }
   }
   }
 }