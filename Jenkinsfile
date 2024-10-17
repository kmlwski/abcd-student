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
        stage('[OSV-Scanner] scan') {
            steps {
                sh '''
                osv-scanner scan --format json --lockfile package-lock.json > ${WORKSPACE}/results/osvscannerResult.xml
                '''
            }
            post {
                always {
                steps {
                    sh '''
                echo 'Hello!'
                '''
                }
            }
        }

    }
}
