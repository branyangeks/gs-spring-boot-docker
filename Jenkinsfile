pipeline {
  agent //any
 {
   kubernetes {
    label 'jenkinskube'
   }
 }
  tools {
    maven 'maven_3.5.2'
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
            sh "mvn clean package -Pminify-js "
        }
    }

    stage('Docker Build'){
        steps{
            echo "Running ${VERSION} on ${env.JENKINS_URL}"
            sh 'docker build -t agiletrailblazers.jfrog.io/demorepo/agiletrailblazers:${BUILD_NUMBER} .'
    }
    }
    stage('Docker Push') {
        steps{
            withDockerRegistry(credentialsId: '272b16c3-ab62-4cfe-a240-703c482788ec', url: 'https://agiletrailblazers.jfrog.io/') {
                sh "docker push agiletrailblazers.jfrog.io/demorepo/agiletrailblazers:${BUILD_NUMBER}"
            }
        }
    }
   }
 }
