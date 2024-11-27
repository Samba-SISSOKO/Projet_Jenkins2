pipeline {
    agent any

    environment {
        SONAR_TOKEN = '3496d37c0a7393e0f03517ac76363b88f47e61f8'  // Votre jeton SonarCloud
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Cloner le dépôt
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'  // Compiler le projet
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'  // Lancer les tests
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    // Exécution de l'analyse SonarCloud
                    withCredentials([string(credentialsId: 'sonarcloud-token', variable: 'SONAR_TOKEN')]) {
                        sh """
                            mvn clean verify sonar:sonar \
                            -Dsonar.projectKey=Samba-SISSOKO_Projet_Jenkins2 \
                            -Dsonar.organization=samba \
                            -Dsonar.host.url=https://sonarcloud.io \
                            -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'  // Créer le package
            }
        }
    }
}
