pipeline {
  agent any

  environment {
    DEPLOY_PATH = "/var/www/test-api"
    BUILD_CONFIG = "Release"
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Restore') {
      steps {
        sh 'dotnet restore'
      }
    }

    stage('Build') {
      steps {
        sh "dotnet build -c ${BUILD_CONFIG} --no-restore"
      }
    }

    stage('Publish') {
      steps {
        sh "dotnet publish -c ${BUILD_CONFIG} -o publish --no-build"
      }
    }

    stage('Deploy') {
      steps {
        sh """
          sudo systemctl stop test-api || true
          sudo rm -rf ${DEPLOY_PATH}/*
          sudo cp -r publish/* ${DEPLOY_PATH}/
          sudo systemctl start test-api
        """
      }
    }
  }
}
