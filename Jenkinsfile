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
        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying to Kubernetes"
                script{
                    kubernetesDeploy(configs: "./manifests.postgres-pv.yml", kubeconfigId: "Kubernetes")
                    kubernetesDeploy(configs: "./manifests.postgres-deployment.yml", kubeconfigId: "Kubernetes")
                    kubernetesDeploy(configs: "./manifests.postgres-service.yml", kubeconfigId: "Kubernetes")
                    kubernetesDeploy(configs: "./manifests.app-deployment.yml", kubeconfigId: "Kubernetes")
                    kubernetesDeploy(configs: "./manifests.app-service.yml", kubeconfigId: "Kubernetes")
                }
            }
        }
    }
}
