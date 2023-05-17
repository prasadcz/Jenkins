pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/prasadcz/Jenkins.git'
            }
        }
        
        stage('Pull and tag') {
            steps {
                sh 'docker pull nginx '
                sh 'docker tag nginx:latest 865600225732.dkr.ecr.ap-south-1.amazonaws.com/testrepo1:latest'
            }
        }
        
        stage('Push to ECR') {
            steps {
                    sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 865600225732.dkr.ecr.ap-south-1.amazonaws.com'
                    sh 'docker push 865600225732.dkr.ecr.ap-south-1.amazonaws.com/testrepo1:latest'
                
            }
        }
        
        stage('Deploy to ECS') {
            steps {
                    sh 'ecs-cli configure --region ap-south-1 --cluster Test'
                    sh 'ecs-cli compose --project-name my-nginx-service service up'
            }
        }
    }
}

