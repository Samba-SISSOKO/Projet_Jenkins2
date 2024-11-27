pipeline {
    agent any

    environment {
        // Définir des variables globales pour Maven et Java
        MAVEN_HOME = '/usr/share/maven' // Chemin de Maven sur l'agent Jenkins
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk' // Chemin de Java 17
        PATH = "${MAVEN_HOME}/bin:${JAVA_HOME}/bin:${env.PATH}"
        SONARQUBE_SERVER = 'SonarQube' // Nom de l'instance SonarQube dans Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Cloner le dépôt Git
                echo 'Cloning the repository...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Commande Maven pour compiler le projet
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Commande Maven pour exécuter les tests
                sh 'mvn test'
            }
            post {
                always {
                    // Publier les rapports de test JUnit si disponibles
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                // Exécuter l'analyse SonarQube
                withSonarQubeEnv('SonarQube') { // Nom de l'instance Sonar dans Jenkins
                    sh 'mvn sonar:sonar -Dsonar.projectKey=nom_du_projet'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the application...'
                // Construire le package (génération du fichier .jar)
                sh 'mvn package'
            }
            post {
                success {
                    // Archiver les artefacts générés
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
            // Étapes de nettoyage et débogage
            echo 'Cleaning workspace and checking file status...'

            // Déboguer l'état du workspace avant nettoyage
            echo 'Listing files in the workspace before cleaning:'
            sh 'ls -la'

            // Nettoyer le workspace après chaque exécution
            cleanWs()

            // Déboguer l'état du workspace après nettoyage
            echo 'Workspace cleaned, listing files after cleanup:'
            sh 'ls -la'

            // Nettoyer les fichiers temporaires
            echo 'Cleaning up temporary files...'
        }
    }
}
