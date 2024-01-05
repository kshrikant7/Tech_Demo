pipeline{
    agent any
    environment{
        API = credentials('openai-api-key')
        // KUBECONFIG = credentials('kubeconfig')
    }
    stages{
        stage('Docker Images Build and Push'){
            steps{
                sh 'docker build -t shrikantk7/tech_frontend ./frontend/'
                sh 'docker build --build-arg OPENAI_API_KEY=$API -t shrikantk7/tech_backend ./backend/'

                withCredentials([usernamePassword(credentialsId: 'dockerhub',usernameVariable: 'DOCKER_USERNAME',passwordVariable: 'DOCKER_PASSWORD')]){
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'

                    sh 'docker push shrikantk7/tech_frontend'
                    sh 'docker push shrikantk7/tech_backend'
                }
            }
        }
        stage('Deploy to Kubernetes Using Helm'){
            steps{
                sh 'sudo -u sigmoid minikube start'
                sh 'sudo -u sigmoid helm install frontend ./frontend/'
                sh 'sudo -u sigmoid helm install backend ./backend/'
            }
        }
    }
}