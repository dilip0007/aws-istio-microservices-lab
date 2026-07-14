pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                // git url: 'https://github.com/dilip0007/aws-istio-microservices-lab.git'
            }
        }
        stage('Build & Test') {
            steps {
                echo 'Running unit tests...'
                // This is where we would run 'go test' or 'npm test'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying Helm Chart to EKS Cluster!'
                // sh 'helm upgrade --install online-boutique ./microservices-demo/helm-chart -n default'
            }
        }
    }
    
    post {
        success {
            echo 'Deployment successful! 🎉'
        }
        failure {
            echo 'Pipeline failed! Alerting the team.'
        }
    }
}
