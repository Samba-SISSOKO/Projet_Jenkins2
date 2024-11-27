pipeline {
    agent any

    environment {
        // Vous pouvez définir des variables globales ici
        MAVEN_HOME = 'C:\\Program Files\\Apache\\maven' // Chemin de Maven sur l'agent Jenkins
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-11'   // Chemin de Java
        PATH = "${MAVEN_HOME}\\bin;${JAVA_HOME}\\bin;${env.PATH}"
        SONARQUBE_SERVER = 'SonarQube' // Nom du serveur SonarQube configuré dans Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Cloner le dépôt Git
                checkout scm
            }
        }


        stage('Test') {
            steps {
                echo 'Running tests...'
                // Commande Maven pour exécuter les tests (en utilisant bat sur Windows)
                bat 'mvn test'
            }
            post {
                always {
                    // Publier les rapports de test JUnit
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                // Exécuter l'analyse SonarQube
                withSonarQubeEnv('SonarQube') { // Nom de l'instance Sonar dans Jenkins
                    bat 'mvn sonar:sonar -Dsonar.projectKey=nom_du_projet'
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Commande Maven pour compiler le projet (en utilisant bat sur Windows)
                bat 'mvn clean compile'
            }
        }

        

        stage('Package') {
            steps {
                echo 'Packaging the application...'
                // Construire le package (génération du fichier .jar)
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
            // Nettoyer les fichiers temporaires
            cleanWs()
        }
    }
}
