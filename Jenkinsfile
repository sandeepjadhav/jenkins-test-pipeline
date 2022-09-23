#!/usr/bin/env
pipeline {
  agent any
  environment {
    NODE_VERSION = '14.18.1'
  }
  stages {
    stage('Install dependencies') {
      steps {
        bat 'npm install'
      }
    }
    
      stage ('Promotion') {
            steps {
                rtPromote (
                    //Mandatory parameter
                    serverId: "https://sandeepjadhav.jfrog.io",
                    targetRepo: "api-build",

                    //Optional parameters
                    buildName: 'MK',
                    buildNumber: '48',
                    comment: 'this is the promotion comment',
                    status: 'Released',
                    includeDependencies: true,
                    failFast: true,
                    copy: true
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
