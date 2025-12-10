pipeline {
    agent any

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
                script {
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('LocalSonar') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                              -Dsonar.projectKey=jenkins-sample-pipeline \
                              -Dsonar.sources=.
                        """
                    }
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
