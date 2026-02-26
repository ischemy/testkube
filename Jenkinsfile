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
                        // 1. Download the kubectl binary to the current workspace
                        echo "Downloading kubectl..."
                        sh 'curl -LO "https://dl.k8s.io(curl -L -s https://dl.k8s.io)/bin/linux/amd64/kubectl"'
                        sh 'chmod +x ./kubectl'
                
                        // 2. Run deployment using the local binary (./kubectl)
                        echo "Deploying App B (Server) to Worker 2..."
                        sh "./kubectl apply -f app-b-deployment.yaml"
                
                        echo "Deploying App A (Client) to Worker 1..."
                        sh "./kubectl apply -f app-a-deployment.yaml"
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
