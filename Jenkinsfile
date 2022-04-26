pipeline{
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                sh "echo staring build the image"
                sh 'docker build -t harshapatel/nodejsapp-1.0:latest .'
            }
        }
        stage('Deploy Docker Image') {
            steps {
                sh "echo staring deploy the image"
                sh 'docker login -u $harshapatel -p $docker@12420'
                sh 'docker push harshapatel/nodejsapp-1.0:latest'
            }
        }
        stage('Remove Docker Image') {
            steps {
                sh "docker rmi -f viraj5132/nodejsapp-1.0"    
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
