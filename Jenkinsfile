pipeline{
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }
    stages{
        stage('Clone Repository'){
            steps{
                git branch: 'master', url: 'https://github.com/ramnathnayak/todo-application.git'
            }
        }

        stage('Build with Maven'){
            steps{
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image'){
            steps{
                sh 'docker build -t todo-application-image:latest .'
            }
        }

        stage('Push Docker Images to Docker Hub'){
            steps{
                sh 'docker login -u $DOCKER_HUB_CREDENTIALS_USR -p $DOCKER_HUB_CREDENTIALS_PSW'
                sh 'docker tag todo-application-image:latest ramnathnayak/todo-application:latest'
                sh 'docker push ramnathnayak/todo-application:latest'
            }
        }

        stage('Deploy with Docker Compose'){
            steps{
                sh 'docker compose up -d'
            }
        }

        stage('Verify Services'){
            steps{
                sh 'docker ps'
            }
        }

        stage('Clean Workspace'){
            steps{
                sh 'rm -rf *'
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment Successful'
        }
        failure{
            echo 'Build or Deployment failed!'
        }
        
    }
}
