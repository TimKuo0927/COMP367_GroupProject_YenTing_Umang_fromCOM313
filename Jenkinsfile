pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install') {
            steps {
                bat 'npm install'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                bat """
                    npx sonar-scanner ^
                    -Dsonar.projectKey=TimKuo0927_COMP367_GroupProject_YenTing_Umang_fromCOM313 ^
                    -Dsonar.organization=comp367-groupproject ^
                    -Dsonar.sources=src ^
                    -Dsonar.host.url=https://sonarcloud.io ^
                    -Dsonar.login=%SONAR_TOKEN%
                """
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Test') {
            steps {
                bat 'npm run test -- --coverage'
            }
            post {
                always {
                    echo 'Test stage finished (even if failed)'
                }
            }
        }
    }
}