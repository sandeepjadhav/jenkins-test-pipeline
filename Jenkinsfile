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
    
      
     stage('Artifactory configuration') {
            steps {
                rtServer(
                        id: "node_app_test",
                           url: 'https://sandeepjadhav.jfrog.io/artifactory',
                      // If you're using username and password:
                      username: 'sandeep_test',
                      password: 'Password@123',
                )
            }
        }
    
    
    stage('Add build trigger') {
            steps {
                rtBuildTrigger(
                        serverId: "node_app_test",
                        spec: "*/10 * * * *",
                       buildName: `test_${currentBuild.number}`,
                      buildNumber: ${currentBuild.number},
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
