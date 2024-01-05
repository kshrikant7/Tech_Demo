pipeline{
    agent any
    environment{
        API = credentials('openai-api-key')
        // KUBECONFIG = credentials('kubeconfig')
    }
    stages{
        stage('Docker Images Build and Push'){
            steps{
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        def frontendImage = docker.build("shrikantk7/tech_frontend", "./frontend/")
                        frontendImage.push()

                        def backendImage = docker.build("shrikantk7/tech_backend", "--build-arg OPENAI_API_KEY=$API ./backend/")
                        backendImage.push()
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes Using Helm'){
            steps{
                sh 'minikube start'
                sh 'helm install frontend ./frontend/'
                sh 'helm install backend ./backend/'
            }
        }
    }
}