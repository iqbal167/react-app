pipeline {
  agent {
      docker {
          image 'node:16-buster-slim'
          args '-p 3000:3000'
      }
  }

  environment {
    VERCEL_TOKEN = credentials('vercel-token')
    APP_URL = "react-app-v2"
  }

  stages {
      stage('Verify Vercel CLI') {
          steps {
              sh 'npx vercel --version'
          }
      }
      
      stage('Pull') {
          steps {
              sh 'npx vercel --token $VERCEL_TOKEN pull --yes'
          }
      }

      stage('Manual Approval') {
          steps {
              input message: 'Lanjutkan ke tahap Deploy?'
          }
      }

      stage('Deploy') { 
          steps {
              sh 'npx vercel --token $VERCEL_TOKEN deploy '
              sleep(time: 1, unit: 'MINUTES')
          }
      }
  }
  post { 
    cleanup { 
      cleanWs()
    }
  }
}