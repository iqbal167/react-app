pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }

    environment {
      VERCEL_TOKEN=credentials('vercel_token')
    }

    stages {
        stage('Pull') {
            steps {
                sh 'vercel --token $VERCEL_TOKEN pull --yes'
            }
        }

        stage('Manual Approval') {
            steps {
                input message: 'Lanjutkan ke tahap Deploy?'
            }
        }

        stage('Deploy') { 
            steps {
                sh 'vercel --token $VERCEL_TOKEN  deploy'
                sleep(time: 1, unit: 'MINUTES')
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}