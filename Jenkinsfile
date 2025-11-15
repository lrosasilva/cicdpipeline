pipeline {
    agent {
        docker { image 'python:3.12' }
    }
    environment {
        WEBEX_TOKEN   = credentials('WEBEX_TOKEN')
        WEBEX_ROOM_ID = credentials('WEBEX_ROOM_ID')
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/lrosasilva/cicdpipeline'
            }
        }

        stage('Build') {
            steps {
                sh 'pip install --upgrade pip'
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }

        stage('Run Sample Code') {
            steps {
                sh 'python3 sample_code.py'
            }
        }

        stage('Test') {
            steps {
                sh 'chmod +x test.sh'
                sh './test.sh'
            }
        }
    }

    post {
        success {
            script {
                sendToWebex("✅ Jenkins Build SUCCESS for ${env.JOB_NAME} #${env.BUILD_NUMBER}")
            }
        }
        failure {
            script {
                sendToWebex("❌ Jenkins Build FAILED for ${env.JOB_NAME} #${env.BUILD_NUMBER}")
            }
        }
    }
}
