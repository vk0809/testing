pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-southeast-2'
        ECR_REPO = '364073565976.dkr.ecr.ap-southeast-2.amazonaws.com/flask-app'
    }
    stages {
        stage ('Build Docker Image') {
            steps {
                sh 'docker build -t flask-app .'
            }
        }
        stage ('Tag Image') {
            steps {
                sh 'docker tag flask-app $ECR_REPO:latest'
            }
        }
        stage ('login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region ap-southeast-2 | docker login --username AWS --password-stdin 364073565976.dkr.ecr.ap-southeast-2.amazonaws.com
                '''
            }
        }
        stage('Deploy to ECS') {
            steps {
                sh '''
                aws ecs update-service \
                --cluster flask-cluster \
                --service flask-service \
		--region ap-southeast-2 \
                --force-new-deployment
                '''
            }
        }
    }
}
