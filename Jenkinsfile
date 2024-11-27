pipeline {
    agent any

    environment {
        MAVEN_HOME = 'C:\\apache-maven-3.8.7-bin\\apache-maven-3.8.7'  // Chemin de Maven
        JAVA_HOME = 'C:\\Program Files\\OpenLogic\\jdk-17.0.13.11-hotspot'  // Chemin de Java
        PATH = "${MAVEN_HOME}\\bin;${JAVA_HOME}\\bin;${env.PATH}"  // Ajouter Maven et Java au PATH
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Exécuter Maven pour compiler le projet
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Exécuter les tests Maven
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
