pipeline {
    agent any

    environment {
        // Assurez-vous que ces chemins sont corrects pour votre machine Windows
        MAVEN_HOME = 'C:\\Program Files\\Apache\\maven\\apache-maven-3.x.x'  // Mettez à jour ce chemin avec votre installation Maven
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'  // Mettez à jour ce chemin avec votre installation Java
        PATH = "${MAVEN_HOME}\\bin;${JAVA_HOME}\\bin;${env.PATH}" // Ajouter Maven et Java au PATH
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Utilisation de la commande bat sous Windows
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Utilisation de la commande bat sous Windows
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

