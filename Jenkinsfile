pipeline {
    agent any
    stages{
        stage('Build'){
            steps{
                 bat '/home/tounga/maven3/bin/mnv test'
            }
        }

        stage('Test'){
        steps{
            bat '/home/tounga/maven3/bin/mnv test'
        }
    }
    }
}
