pipeline {
    agent any

    tools {
        maven 'Maven' // Nom configuré dans "Global Tool Configuration"
    }

    environment {
        JAVA_HOME = 'C:\\Program Files\\OpenLogic\\jdk-17.0.13.11-hotspot' // Remplacez par le chemin correct
        PATH = "${JAVA_HOME}\\bin:${env.PATH}"
        SONARQUBE_TOKEN = 'dcac9bc6971662ece67c67fb68b6d7a419cfc9d1'  // Remplacez par votre jeton SonarQube
        SONARQUBE_URL = 'http://localhost:8080' // L'URL de votre serveur SonarQube
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml' // Publie les résultats des tests
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                bat '''
                    mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=Samba-SISSOKO_Projet_Jenkins2 \
                    -Dsonar.organization=samba \
                    -Dsonar.host.url=${SONARQUBE_URL} \
                    -Dsonar.login=${SONARQUBE_TOKEN}
                '''
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the application...'
                bat 'mvn package'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            cleanWs() // Nettoie les fichiers temporaires
        }
    }
}
