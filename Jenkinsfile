pipeline {
    agent any

    environment {
        dockerImage = ''
        registry = 'eukhlg/django'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
