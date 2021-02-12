pipeline {
  agent 
//  {
//    kubernetes {
//     label 'jenkinskube'
//    }
//  }
  tools {
    maven 'maven_3.6.3'
    }
  environment{
    DOCKER_HOME = tool name: 'docker_19_03_6', type: 'dockerTool'
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
            sh "mvn package spring-boot:run"
        }
    }

    // stage('Docker Build'){
    //     steps{
    //         echo "Running ${VERSION} on ${env.JENKINS_URL}"
    //         sh 'docker build -t agiletrailblazers.jfrog.io/demorepo/agiletrailblazers:${BUILD_NUMBER} .'
    // }
   // }
    stage('Docker Push') {
        steps{
            withDockerRegistry(credentialsId: 'aksregistrykey', url: 'aksregistryuseast.azurecr.io') {
              sh "mvn compile jib:build"
                // sh "docker push agiletrailblazers.jfrog.io/demorepo/agiletrailblazers:${BUILD_NUMBER}"
            
            }
        }
    }
   }
 }
