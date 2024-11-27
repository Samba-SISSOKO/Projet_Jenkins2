pipeline {
    agent any

    environment {
        // Définir des variables d'environnement globales
        MAVEN_HOME = '/usr/share/maven'  // Chemin de Maven sur l'agent Jenkins
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk'  // Chemin de Java
        PATH = "${MAVEN_HOME}/bin:${JAVA_HOME}/bin:${env.PATH}"
        SONARQUBE_SERVER = 'SonarQube'  // Nom de l'instance SonarQube dans Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    try {
                        echo "Cloning the repository from GitHub..."
                        // Cloner le dépôt depuis GitHub, sur la branche master
                        checkout([$class: 'GitSCM', 
                                  branches: [[name: 'refs/heads/master']],  // Spécifier la branche master
                                  userRemoteConfigs: [[url: 'https://github.com/Samba-SISSOKO/Projet_Jenkins2']]])  // URL du dépôt public
                    } catch (Exception e) {
                        echo "Checkout failed: ${e.getMessage()}"
                        throw e  // Relance l'erreur pour que le pipeline échoue ici
                    }
                }
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
