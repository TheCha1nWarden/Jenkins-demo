pipeline {
    agent any
    environment {
        K8S_TOKEN = credentials('K8S_TOKEN')  // Используем секрет из Jenkins Credentials
        K8S_SERVER = 'https://kubernetes.default.svc'  // API-сервер внутри кластера
        NAMESPACE = 'default'
    }
    stages {
        stage('Setup kubectl') {
            steps {
                script {
                    sh """
                    echo "Configuring kubectl..."
                    kubectl config set-cluster k8s-cluster --server=$K8S_SERVER --insecure-skip-tls-verify=true
                    kubectl config set-credentials jenkins --token=$K8S_TOKEN
                    kubectl config set-context k8s-context --cluster=k8s-cluster --user=jenkins --namespace=$NAMESPACE
                    kubectl config use-context k8s-context
                    """
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh """
                    echo "Deploying to Kubernetes..."
                    kubectl apply -f k8s/deployment.yaml
                    kubectl apply -f k8s/service.yaml
                    kubectl apply -f k8s/ingress.yaml
                    """
                }
            }
        }
    }
}