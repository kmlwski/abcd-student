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
                semgrep scan --config auto --json > ${WORKSPACE}/results/SemgrepResult.json || true
                '''
            }
            post {
            always {
                defectDojoPublisher(artifact: '${WORKSPACE}/results/SemgrepResult.json', 
                    productName: 'Juice Shop', 
                    scanType: 'Semgrep JSON Report',
                    engagementName: 'kml.wski@gmail.com')
            }
            }
        }
}
}