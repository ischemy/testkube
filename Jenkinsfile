pipeline {
    agent any

    environment {
        // ID kredensial yang dibuat di Jenkins
        KUBECONFIG_CREDENTIAL_ID = 'k8s-config'
    }

    stages {
        stage('Checkout Source') {
            steps {
                // Mengambil manifest dari repository Git Anda
                checkout scm
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Menggunakan plugin Kubernetes CLI untuk auth
                withKubeConfig([credentialsId: "${KUBECONFIG_CREDENTIAL_ID}"]) {
                    script {
                        echo "Deploying App B (Server) to Worker 2..."
                        sh "kubectl apply -f app-b-deployment.yaml"
                        
                        echo "Deploying App A (Client) to Worker 1..."
                        sh "kubectl apply -f app-a-deployment.yaml"
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                withKubeConfig([credentialsId: "${KUBECONFIG_CREDENTIAL_ID}"]) {
                    sh "kubectl get pods -o wide"
                    echo "Pastikan pod app-a dan app-b berada di NODE yang berbeda."
                }
            }
        }
    }
}
