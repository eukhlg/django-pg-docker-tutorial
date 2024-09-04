pipeline {
    agent any

    environment {
        dockerImage = ''
        registry = 'eukhlg/django'
        registryCredential = 'Dockerhub'
    }
    
    stages {
        stage('Build Docker Image') {
            steps {
                
                echo "Building Docker Image..."
                script {
                    dockerImage = docker.build registry
                }

            }
        }
        stage('Push Docker Image') {
            steps {
                script{
                    
                    echo "Pushing Docker Image..."
                    docker.withRegistry( '', registryCredential ){
                        dockerImage.push("${env.TAG_NAME}")
                        dockerImage.push('latest')
                    }
                    
                }
            }
        }
        /*
        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying to Kubernetes"
                script{
                    
                    withKubeConfig([credentialsId: 'Kubernetes', serverUrl: 'https://172.20.1.14:6443']) {
                    sh 'kubectl apply -f ./manifests/ --validate=false'

                    }
      
                }
            } 
        } */
        
        stage('Helm Lint') {
            steps {
                echo "Linting the Helm chart to ensure there are no issues..."
                sh 'helm lint ./chart'
            }
        }

        stage('Deploy Helm Chart') {
            steps {
                
                echo "Deploying the Helm chart..."
                withKubeConfig([credentialsId: 'Kubernetes', serverUrl: 'https://172.20.1.14:6443']) {
                sh 'helm upgrade --install django ./chart --values ./chart/values.yaml'
                
                }
            }           
        }
    }
}