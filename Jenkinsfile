pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token')
        GITHUB_TOKEN = credentials('github-pat')
    }

    triggers {
        pollSCM('H/5 * * * *')
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
                bat 'npm run coverage'
            }
            post {
                always {
                    echo 'Test stage finished (even if failed)'
                }
            }
        }

        stage('Deliver') {
            steps {
                bat 'echo Releasing artifact...'
            }
        }

        stage('Deploy to GitHub Pages(Dev)') {
            steps {
                echo 'Deploying to GitHub Pages(Dev)...'
                bat 'set GITHUB_TOKEN=%GITHUB_TOKEN% && npm run deploy'
            }
        }


        stage('Deploy to QAT') {
            steps {
                echo 'Deploying to QAT environment...'
                bat 'echo App deployed to QAT'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging environment...'
                bat 'echo App deployed to STAGING'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to Production environment...'
                bat 'echo App deployed to PRODUCTION'
            }
        }
    }
}