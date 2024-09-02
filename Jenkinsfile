pipeline {
    agent any

    environment {
        dockerImage = ''
        registry = 'eukhlg/django'
        registryCredential = 'Dockerhub'
        kubeCA = '-----BEGIN CERTIFICATE-----
MIIDBTCCAe2gAwIBAgIIJScAyMY6vI8wDQYJKoZIhvcNAQELBQAwFTETMBEGA1UE
AxMKa3ViZXJuZXRlczAeFw0yNDA5MDExNTA5MzBaFw0zNDA4MzAxNTE0MzBaMBUx
EzARBgNVBAMTCmt1YmVybmV0ZXMwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEK
AoIBAQCz7FEretTk00K3YLjha6VImMhiKck84CvuAujrIVPfZES493+aeHMvWzAG
Im226ZC+b0+7L8hE3HmgvRB9QWul4deoB65xqHUx/41kZeLM/qXqG3vMhL8+ADoH
b/euo6qaJ07da0pu9mKXTAK8yTBWs9kfqS3Ce0DzCNXyByeVuY8vr5u2ApN0Yhox
mJoZ2S7M9eTROUqaWfjrO/dabn9fpWiddt/x/SYTs43Elf1aat4L208Y6WQ+6Q4a
60FphaQ4Li7WIswUqe+SMLgDfvWxr7AH1iXxHOiwFhzv8rcK448aHEI0oiMDe3aQ
JkjObxZZJCuf0yz2UQYn3mMWoig5AgMBAAGjWTBXMA4GA1UdDwEB/wQEAwICpDAP
BgNVHRMBAf8EBTADAQH/MB0GA1UdDgQWBBSqjSmVeFkpy9lalU0JOKMsVglEETAV
BgNVHREEDjAMggprdWJlcm5ldGVzMA0GCSqGSIb3DQEBCwUAA4IBAQAJvkGJj6fH
m4Gx+ble/odRVUmzJzDKit+P+536ClOJlrrN+XIBBwzspIoBMXIjgo+coE3f8veQ
TW493MOhvSEgf84+2X058GqEWiSMnRb3lCGtBq+8q/6EpCbfyYy61wG8TbEyPcMc
WAvRgF2lhR8M8Q2DiuzTfZuKn1sPjJDuQH8EibwN/4pjJ8JNZkmWjfKDqyBQQEwY
sqYeZ4IdcLAhLXhvITg4a7IBnGiql0ouFpGRPKhRrG57pgVJsl5SgpyNZKnWMrvp
gZAAJjIem+Pc+a4xXW3o7gFwcFxJ6nNZUXnKMdcV948rEEMrZ/s+R0WaNgq1NEqn
FWzYv0/gG6/w
-----END CERTIFICATE-----'
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
                    withKubeConfig([credentialsId: 'Kubernetes', serverUrl: 'https://172.20.1.14:6433', caCertificate: kubeCA, namespace: "jenkins"]) {
                    sh 'kubectl get configmap'
                    }
      
                }
            }
        }
    }
}
