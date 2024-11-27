pipeline {
    agent any

    environment {
        // Utilisation de l'outil Java 17 configuré dans Jenkins
        JAVA_HOME = tool name: 'JDK 17', type: 'JDK'  // Nom de l'installation de Java dans Jenkins
        MAVEN_HOME = '/usr/share/maven' // Si Maven est installé sur votre machine
        PATH = "${MAVEN_HOME}/bin:${JAVA_HOME}/bin:${env.PATH}"
        SONARQUBE_SERVER = 'SonarQube' // Nom du serveur SonarQube configuré dans Jenkins
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
                sh 'mvn clean compile -Dmaven.compiler.source=17 -Dmaven.compiler.target=17'
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
