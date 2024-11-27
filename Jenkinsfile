pipeline {
    agent any

    environment {
        // Assurez-vous que la version de Java correspond à Java 17
        JAVA_HOME = tool name: 'JDK 17', type: 'JDK' // 'JDK 17' doit correspondre au nom de l'installation dans Jenkins
        MAVEN_HOME = '/usr/share/maven' // Chemin de Maven sur l'agent Jenkins
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
