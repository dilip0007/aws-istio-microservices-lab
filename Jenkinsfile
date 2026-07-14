def services = ['adservice', 'cartservice', 'checkoutservice', 'currencyservice', 'emailservice', 'frontend', 'loadgenerator', 'paymentservice', 'productcatalogservice', 'recommendationservice', 'shippingservice']

pipeline {
    agent {
        kubernetes {
            yaml '''
            apiVersion: v1
            kind: Pod
            spec:
              containers:
              - name: kaniko
                image: gcr.io/kaniko-project/executor:debug
                command:
                - sleep
                args:
                - 9999999
                volumeMounts:
                - name: docker-config
                  mountPath: /kaniko/.docker/
              - name: helm
                image: alpine/helm:3.12.0
                command:
                - sleep
                args:
                - 9999999
              volumes:
              - name: docker-config
                secret:
                  secretName: docker-credentials
                  items:
                    - key: config.json
                      path: config.json
            '''
        }
    }
    
    stages {
        stage('Build and Push Images (Parallel)') {
            steps {
                script {
                    def parallelStages = [:]
                    
                    for (int i = 0; i < services.size(); i++) {
                        def svc = services[i]
                        parallelStages[svc] = {
                            stage("Build ${svc}") {
                                container('kaniko') {
                                    // Use Kaniko to build the image inside the pod without a Docker daemon!
                                    sh "/kaniko/executor --context ${WORKSPACE}/src/${svc} --dockerfile ${WORKSPACE}/src/${svc}/Dockerfile --destination dilipnigam007/${svc}:v${env.BUILD_NUMBER}"
                                }
                            }
                        }
                    }
                    
                    // Execute all 11 builds concurrently!
                    parallel parallelStages
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                container('helm') {
                    echo "Deploying version v${env.BUILD_NUMBER} to Kubernetes..."
                    // Tell Helm to pull the images from the user's personal Docker Hub using the new tags
                    sh "helm upgrade --install online-boutique ./helm-chart -n default --set images.repository=docker.io/dilipnigam007 --set images.tag=v${env.BUILD_NUMBER}"
                }
            }
        }
    }
    
    post {
        success {
            echo 'Massive Monorepo Deployment successful! 🎉'
        }
        failure {
            echo 'Pipeline failed! Check the Kaniko build logs.'
        }
    }
}
