#!/usr/bin/env
pipeline {
  agent any
  environment {
    NODE_VERSION = '14.18.1'
  }
  stages {
     stage('Print hello') {
      steps {
        echo 'Hello world!'
      }
    }
    stage('Install dependencies') {
      steps {
        echo 'node -v'
        bat 'npm install'
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
