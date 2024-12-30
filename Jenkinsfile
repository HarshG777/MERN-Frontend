pipeline {
    agent any

    tools{
        Nodejs 'sonarnode'
    }

    environment {
        NODEJS_HOME = 'C:\\Program Files\\nodejs'  // Replace with your configured Node.js tool name in Jenkins
        SONAR_SCANNER_PATH = 'C:\\Users\\lenovo\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
    }

    stages {
        stage('CheckOut') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm install
                '''
            }
        }

        stage('Lint'){
            steps{
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run lint
                '''
            }
        }

        stage('Build') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run build
                '''
            }
        }

        stage('SonarAnalysis') {
            environment {
                SONAR_TOKEN = credentials('sonarQub-token') // SonarQube token stored in Jenkins credentials
            }
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                sonar-scanner.bat 
                -Dsonar.projectKey=MERN-frontEnd 
                -Dsonar.sources=. 
                -Dsonar.host.url=http://localhost:9000
                -Dsonar.login=%SONAR_TOKEN%
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed. Please check the logs."
        }
    }
}
