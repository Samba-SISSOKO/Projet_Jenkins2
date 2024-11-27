pipeline {
    agent any

    environment {
        // Vous pouvez définir des variables globales ici
        MAVEN_HOME = '/usr/share/maven' // Chemin de Maven sur l'agent Jenkins
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk' // Chemin de Java
        PATH = "${MAVEN_HOME}/bin:${JAVA_HOME}/bin:${env.PATH}"
        SONARQUBE_SERVER = 'samba' // Nom du serveur SonarQube configuré dans Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Cloner le dépôt Git
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
                    // Publier les rapports de test JUnit
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                // Exécuter l'analyse SonarQube
                withSonarQubeEnv('samba') { // Nom de l'instance Sonar dans Jenkins
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
