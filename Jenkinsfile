pipeline {
    agent any

    environment {
        dockerImage = ''
        registry = 'eukhlg/django'
        registryCredential = 'Dockerhub'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
                script{
                    docker.withRegistry( '', registryCredential ){
                        dockerImage.push()
                    }
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
