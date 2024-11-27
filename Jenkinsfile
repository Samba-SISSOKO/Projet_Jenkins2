                   pipeline {
                       agent any

                       environment {
                           SONARQUBE_TOKEN = credentials('sonarcloud-token')  // Token pour l'authentification
                       }

                       stages {
                           stage('Checkout') {
                               steps {
                                   checkout scm  // Cloner le dépôt
                               }
                           }

                           stage('Build') {
                               steps {
                                   sh 'mvn clean compile'  // Compiler le projet
                               }
                           }

                           stage('Test') {
                               steps {
                                   sh 'mvn test'  // Lancer les tests
                               }
                           }

                           stage('Code Quality Analysis') {
                               steps {
                                   script {
                                       // SonarCloud analysis
                                       withCredentials([string(credentialsId: 'sonarcloud-token', variable: 'SONAR_TOKEN')]) {
                                           sh "mvn clean verify sonar:sonar -Dsonar.projectKey=your_project_key -Dsonar.organization=your_organization -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=${SONAR_TOKEN}"
                                       }
                                   }
                               }
                           }

                           stage('Package') {
                               steps {
                                   sh 'mvn package'  // Créer le package
                               }
                           }
                       }
                   }
