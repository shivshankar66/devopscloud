pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "shivam193/cloudanalogy:latest"
    }

    stages {

        stage('Clone Code') {
            steps {
                git credentialsId: 'github-creds', url: 'https://github.com/shivshankar66/devopscloud.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
                sh 'kubectl rollout restart deployment react-app'
            }
        }
    }

    post {
        success {
            echo "✅ Build Success 🚀"
        }
        failure {
            echo "❌ Build Failed"
        }
    }
}
