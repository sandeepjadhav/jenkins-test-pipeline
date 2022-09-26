#!/usr/bin/env
pipeline {
  agent any
  environment {
    NODE_VERSION = '14.18.1'
    SERVER_ID = 'nodeAppID'
    ARTIFCTORY_URL: 'https://sandeepjadhav.jfrog.io/artifactory'
    ARTIFCTORY_USER_NAME: 'admin'
    ARTIFCTORY_PASSWORD: 'Password@123'
  }
  stages {
    stage('Install dependencies') {
      steps {
        bat 'npm install'
      }
    }
    
      
     stage('Artifactory configuration') {
            steps {
                rtServer(
                       id: SERVER_ID,
                       url: ARTIFCTORY_URL,
                      // If using username and password:
                      username: ARTIFCTORY_USER_NAME,
                      password: ARTIFCTORY_PASSWORD,
                )
            }
        }
    
    
    stage ('Upload') {
            steps {
                rtUpload (
                    buildName: "${env.BUILD_NUMBER}",
                    buildNumber: "${env.BUILD_NUMBER}",
                    serverId: SERVER_ID,
                  spec: """{
                                "files": [
                                    {
                                    "pattern": "*.zip",
                                    "target": "jenkin_node_test/"
                                    }
                                ]
                            }"""
                )
            }
        }
  
    

  }
  
  
  post {
    failure {
      echo 'Processing failed'
    }
    success {
      echo 'Processing succeeded'
    }
  }
}
