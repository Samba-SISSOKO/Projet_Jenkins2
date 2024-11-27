pipeline {
    agent any
    tools {
        maven 'Maven' // Utilisez le même nom que celui défini dans "Global Tool Configuration"
    }
    stages {
        stage('Build') {
            steps {
                bat 'mvn clean compile'
            }
        }
    }
}
