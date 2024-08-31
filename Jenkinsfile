pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'echo $PWD | docker login -u $UNAME --password-stdin'
                sh 'docker build -t eukhlg/django .'
                sh "docker push eukhlg/django:${TAG_NAME}"
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
