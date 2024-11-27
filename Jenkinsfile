pipeline {
    agent any

    tools {
        // Assurez-vous que Maven et Java sont correctement configurés dans Jenkins
        maven 'Maven'  // Nom de l'installation Maven dans "Global Tool Configuration"
        jdk 'JDK11'    // Nom de l'installation JDK dans "Global Tool Configuration"
    }

    stages {
        stage('Preparation') {
            steps {
                echo 'Preparation: Nettoyage du workspace'
                bat 'del /q /s *.* || echo Rien à supprimer'
            }
        }

        stage('Build') {
            steps {
                echo 'Etape de Build: Compilation du projet'
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Etape de Test: Exécution des tests unitaires'
                bat 'mvn test'
            }
        }

        stage('Code Quality Analysis') {
            steps {
                echo 'Analyse de la qualité de code avec SonarQube'
                withSonarQubeEnv('SonarQube') { // Remplacez 'SonarQube' par le nom de votre serveur dans Jenkins
                    bat 'mvn sonar:sonar -Dsonar.projectKey=your-project-key'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Etape de Packaging: Création du package JAR/WAR'
                bat 'mvn package'
            }
        }

        stage('Archive') {
            steps {
                echo 'Archivage des fichiers générés'
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline terminé avec succès.'
        }
        failure {
            echo 'Pipeline échoué.'
        }
    }
}
