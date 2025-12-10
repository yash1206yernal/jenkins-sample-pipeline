pipeline {
    agent any

    tools {
        sonarQubeScanner 'sonar-scanner'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
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

        stage('Code Quality Scan') {
            steps {
                withSonarQubeEnv('LocalSonar') {
                    sh '''
                        sonar-scanner \
                        -Dsonar.projectKey=jenkins-sample-pipeline \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://192.168.0.24:9000
                    '''
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '*.txt', fingerprint: true
        }
    }
}
