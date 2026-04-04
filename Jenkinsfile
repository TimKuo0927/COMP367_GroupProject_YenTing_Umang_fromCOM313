pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token')
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
        script {
            withCredentials([string(credentialsId: 'github-pat1', variable: 'TOKEN')]) {
                bat """
                    :: 1. Force Git to accept long paths
                    git config --global core.longpaths true
                    
                    :: 2. Run deploy using a custom (shorter) cache folder
                    set DEBUG=gh-pages
                    npx gh-pages -d dist -r https://%TOKEN%@github.com/TimKuo0927/COMP367_GroupProject_YenTing_Umang_fromCOM313.git --dest .
                """
            }
        }
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
