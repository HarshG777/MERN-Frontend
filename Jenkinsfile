pipeline {
    agent any

    environment {
        NODE_HOME = tool 'nodejs' // Replace with your configured Node.js tool name in Jenkins
        PATH = "${NODE_HOME}/bin:${env.PATH}"
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
                npm install
                '''
            }
        }

        stage('Run Tests') {
            steps {
                bat '''
                npm test
                '''
            }
        }

        stage('SonarAnalysis') {
            environment {
                SONAR_TOKEN = credentials('sonarQub-token') // SonarQube token stored in Jenkins credentials
            }
            steps {
                bat '''
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
