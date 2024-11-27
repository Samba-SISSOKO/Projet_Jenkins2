pipeline {
    agent any

    environment {
        // Définir les variables d'environnement Maven et Java pour Windows
        MAVEN_HOME = 'C:\\Program Files\\Maven'  // Chemin vers l'installation Maven sur Windows
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'  // Chemin vers l'installation Java sur Windows
        PATH = "${MAVEN_HOME}\\bin;${JAVA_HOME}\\bin;${env.PATH}" // Ajouter Maven et Java au PATH
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Utilisez la commande bat sous Windows
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Utilisez la commande bat pour exécuter les tests Maven
                bat 'mvn test'
            }
        }
    }

    post {
        success {
            echo 'Build and tests succeeded!'
        }
        failure {
            echo 'Build or tests failed!'
        }
    }
}
