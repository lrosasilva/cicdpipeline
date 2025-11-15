def sendToWebex(String msg) { 
    sh """
        curl -X POST \
        -H "Authorization: Bearer ${WEBEX_TOKEN}" \
        -H "Content-Type: application/json" \
        -d '{ "roomId": "${WEBEX_ROOM_ID}", "text": "${msg}" }' \
        https://webexapis.com/v1/messages
    """
}

pipeline {
    agent any

    environment {
        WEBEX_TOKEN   = credentials('WEBEX_TOKEN')     // Your WebEx bot token
        WEBEX_ROOM_ID = credentials('WEBEX_ROOM_ID')   // Your WebEx space ID
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/lrosasilva/cicdpipeline'
            }
        }

        stage('Build') {
            steps {
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }

        stage('Run Sample Code') {
            steps {
                echo "Running sample_code.py..."
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
                sendToWebex("JJenkins Build SUCCESS for ${env.JOB_NAME} #${env.BUILD_NUMBER}")
            }
        }
        failure {
            script {
                sendToWebex("Jenkins Build FAILED for ${env.JOB_NAME} #${env.BUILD_NUMBER}")
            }
        }
    }
}
