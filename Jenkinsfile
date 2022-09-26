#!/usr/bin/env
pipeline {
  agent any
  environment {
    NODE_VERSION = '14.18.1'
    SERVER_ID = 'nodeAppID'
    // This needs to be filled with respecitve data
    ARTIFCTORY_URL= 'https://sandeepjadhav.jfrog.io/artifactory'
    ARTIFCTORY_USER_NAME= 'admin'
    ARTIFCTORY_PASSWORD= 'Password@123'
    ARTIFCTORY_NAME = 'jenkin_node_test'
  }
  stages {
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
                "target": "${ARTIFCTORY_NAME}/"
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
