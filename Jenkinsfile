pipeline {
    agent any
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
            def message = "✅ Jenkins Build SUCCESS for ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            sendToWebex(message)
        }
    }
    failure {
        script {
            def message = "❌ Jenkins Build FAILED for ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            sendToWebex(message)
        }
    }
}

def sendToWebex(String msg) {
    def webhookUrl = 'https://webexapis.com/v1/messages'
    def token = 'Y2YyY2E5NzEtMTZkMC00YjQzLWE4ZjYtZGU0YzYwODA4NzgxMzVhMzNjMjctZmM0_P0A1_e58072af-9d57-4b13-abf7-eb3b506c964d'
    def roomId = 'Y2lzY29zcGFyazovL3VybjpURUFNOnVzLXdlc3QtMl9yL1JPT00vNDFjZjQ3ZTAtYTY2MC0xMWYwLTlkY2EtNDc1ZDUzMDg2NjM3'
    sh """
        curl -X POST "${webhookUrl}" \
        -H "Authorization: Bearer ${token}" \
        -H "Content-Type: application/json" \
        -d '{"roomId": "${roomId}", "text": "${msg}"}'
    """
}

}
