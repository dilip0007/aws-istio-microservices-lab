pipeline {
    agent any
    
    environment {
        // Adding our local bin directory to the PATH so we can execute helm
        PATH = "$WORKSPACE/bin:$PATH"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                git url: 'https://github.com/dilip0007/aws-istio-microservices-lab.git', branch: 'main'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo 'Installing Helm...'
                sh '''
                    mkdir -p $WORKSPACE/bin
                    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
                    chmod 700 get_helm.sh
                    HELM_INSTALL_DIR=$WORKSPACE/bin ./get_helm.sh --no-sudo
                    helm version
                '''
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying Helm Chart to EKS Cluster!'
                sh 'helm upgrade --install online-boutique ./helm-chart -n default'
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
