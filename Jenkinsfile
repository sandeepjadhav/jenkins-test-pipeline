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
                    serverId: "node_app_test",
                  spec: """{
                                "files": [
                                    {
                                    "pattern": "${env.BUILD_NUMBER}-*.zip",
                                    "target": "api-v/"
                                    }
                                ]
                            }"""
                )
            }
        }

        stage ('Download') {
            steps {
                rtDownload (
                    buildName: "${env.BUILD_NUMBER}",
                    buildNumber: "${env.BUILD_NUMBER}",
                    serverId: "node_app_test" ,
                  spec: """{
                                "files": [
                                     {
      "pattern": "libs-snapshot-local/*(Pipeline).zip",
      "target": "Bazinga/{1}/",
      "props": "p1=v1;p2=v2"
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
                    serverId: "node_app_test"
                )
            }
        }
      
    
    stage('Add build trigger') {
            steps {
                
                 rtPromote (
                    //Mandatory parameter
                    serverId: 'node_app_test',
                     targetRepo: 'test_1_repo',

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
