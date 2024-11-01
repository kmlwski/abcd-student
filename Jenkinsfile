pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Code checkout from GitHub') {
            steps {
                script {
                    cleanWs()
                    git credentialsId: 'github-token', url: 'https://github.com/kmlwski/abcd-student', branch: 'main'
                }
            }
        }
        stage('[Trufflehog] scan') {
            steps {
                sh 'mkdir -p results/'
                sh '''
                trufflehog github --repo=https://github.com/kmlwski/abcd-student --json > ${WORKSPACE}/results/TruffleHogResult.json || true
                '''
            }
            post {
            always {
                defectDojoPublisher(artifact: '${WORKSPACE}/results/TruffleHogResult.json', 
                    productName: 'Juice Shop', 
                    scanType: 'Trufflehog Scan',
                    engagementName: 'kml.wski@gmail.com')
            }
            }
        }
}
}