#!/usr/bin/env
pipeline {
  agent any
  environment {
    NODE_VERSION = '14.18.1'
    SERVER_ID = 'nodeAppID'
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
                           url: 'https://sandeepjadhav.jfrog.io/artifactory',
                      // If you're using username and password:
                      username: 'admin',
                      password: 'Password@123',
                )
            }
        }
    
    
    stage ('Upload') {
            steps {
                rtUpload (
                    buildName: "${env.BUILD_NUMBER}",
                    buildNumber: "${env.BUILD_NUMBER}",
                    // Obtain an Artifactory server instance, defined in Jenkins --> Manage Jenkins --> Configure System:
                    serverId: SERVER_ID,
                  spec: """{
                                "files": [
                                    {
                                    "pattern": "P_new/*",
                                    "target": "nodejenkinapp/uploaded/"
                                    }
                                ]
                            }"""
                )
            }
        }
  stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    buildName: "${env.BUILD_NUMBER}",
                    buildNumber: "${env.BUILD_NUMBER}",
                    serverId: SERVER_ID
                )
            }
        }
      
    
    stage('Add build trigger') {
            steps {
                
                 rtPromote (
                    //Mandatory parameter
                    serverId: SERVER_ID,
                     targetRepo: 'nodejenkinapp',

                    //Optional parameters
                    buildName: "${env.BUILD_NUMBER}",
                    buildNumber: "${env.BUILD_NUMBER}",
                    comment: 'this is the promotion comment',
                    status: 'Released',
                    includeDependencies: true,
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
