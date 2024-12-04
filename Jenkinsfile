
pipeline {
    agent any

    tools {
        maven 'Maven' // Nom configuré dans "Global Tool Configuration"
    }

    environment {
        JAVA_HOME = 'C:\\Program Files\\OpenLogic\\jdk-17.0.13.11-hotspot' // Remplacez par le chemin correct
        PATH = "${JAVA_HOME}\\bin:${env.PATH}"
        SONARQUBE_SERVER = 'SonarQube' // Nom configuré dans Jenkins pour SonarQube
        SONAR_TOKEN = '58d4108073381c479d83056d52c4c0953450aa37' // Votre token d'accès
        SONAR_PROJECT_KEY = 'Samba-SISSOKO_Projet_Jenkins2'
        SONAR_ORG_KEY = 'samba'
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

        stage('SonarQube Analysis') {
            steps {
                script {
                    echo 'Running SonarQube analysis...'
                    // Lancer l'analyse SonarQube
                    bat "mvn sonar:sonar -Dsonar.projectKey=${env.SONAR_PROJECT_KEY} -Dsonar.organization=${env.SONAR_ORG_KEY} -Dsonar.login=${env.SONAR_TOKEN} -Dsonar.host.url=https://sonarcloud.io"
                }
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
