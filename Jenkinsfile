pipeline{
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                sh "echo staring build the image"
                sh 'docker build -t harshapatel/docker-nginx:latest .'
            }
        }
        stage('Deploy Docker Image') {
            steps {
                sh "echo staring deploy the image"
                sh 'docker login -u $DOCKER_ID -p $DOCKER_PASSWORD'
                sh 'docker push harshapatel/docker-nginx:latest'
            }
        }
        stage('Remove Docker Image') {
            steps {
                sh "docker rmi -f harshapatel/docker-nginx"    
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sshagent(['3.91.222.148']) {
                    sh "echo staring deploy the image in Kubernetes"
                    sh "scp -o StrictHostKeyChecking=no nodejsapp.yaml ubuntu@$DEPLOY_IP:/home/ubuntu/"
                    sh "ssh ubuntu@$DEPLOY_IP kubectl apply -f nodejsapp.yaml" 
                }
            }
        }
    
    }
}
