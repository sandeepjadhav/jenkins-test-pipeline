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
                
                 rtPromote (
                    //Mandatory parameter
                    serverId: 'node_app_test',
                    targetRepo: 'https://sandeepjadhav.jfrog.io/artifactory',

                    //Optional parameters
                    buildName: "${env.BUILD_NUMBER}",
                    buildNumber: "${env.BUILD_NUMBER}",
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
