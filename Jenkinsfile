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
                        //echo "Downloading kubectl..."
                        //sh 'curl -LO "https://dl.k8s.io/v1.35.1/bin/darwin/amd64/kubectl (curl -L -s https://dl.k8s.io/v1.35.1/bin/darwin/amd64/kubectl)/bin/linux/amd64/kubectl"'
                        //sh 'chmod +x ./kubectl'
                
                        //echo "Downloading kubectl binary..."
                        // Hardcoding the version avoids shell escaping issues in Jenkins
                        sh 'curl -L "https://dl.k8s.io/v1.32.12/bin/linux/arm64/kubectl " -o kubectl'
                        sh 'chmod +x ./kubectl'
                        
                        echo "Deploying App B..."
                        sh "./kubectl apply -f app-b-deployment.yaml"
                
                        echo "Deploying App A..."
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
