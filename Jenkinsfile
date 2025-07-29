pipeline {
    agent any

    tools {
        jdk 'jdk17'       // Make sure this name matches your Jenkins JDK installation
        nodejs 'node16'   // Ensure NodeJS is installed with this exact name
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner' // Ensure "sonar-scanner" is defined in Global Tool Config
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/thushi99/Netflix-DevSecOps-Project.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') { // 'sonar-server' must match the configured server name
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=Netflix \
                        -Dsonar.projectKey=Netflix \
                        -Dsonar.sources=.'''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: true, credentialsId: 'Sonar-token'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
    }
}
