pipeline {
    agent any

    tools{nodejs "NodeJS"}

    stages {
        stage('Cloning git') {
            steps {
                git 'https://github.com/JusApart/vscode'
            }
        }
        stage('SonarQube') {
            environment {
                scannerHome = tool 'sonarscanner'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '${scannerHome}/bin/sonar-scanner'
                }
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Build') {
            steps {
                sh 'yarn preinstall'
                sh 'yarn'
                sh 'yarn postinstall'
            }
        }
        stage('Test') {
            steps {
                sh 'yarn test-browser'
            }
        }
        stage('Deployment') {
            steps {
                sh 'yarn web'
            }
        }
    }
}
