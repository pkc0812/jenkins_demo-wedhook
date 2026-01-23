pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'pkc0812/jenkins-demo-webhook'
        CONTAINER_NAME = 'pkc08122'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github_jenkins',     // ✅ Your GitHub credential
                    url: 'https://github.com/pkc0812/jenkins_demo-wedhook.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .
                    docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest
                """
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_jenkins', 
                                                 usernameVariable: 'DOCKER_USER', 
                                                 passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                        docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                        docker push ${DOCKER_IMAGE}:latest
                        docker logout
                    """
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    docker run -d -p 8080:80 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}:latest
                    sleep 5
                    curl -f http://localhost:8080 || echo "Health check failed"
                """
            }
        }
    }
    
    post {
        always {
            sh 'docker image prune -f || true'
        }
        success {
            echo "✅ SUCCESS! App live at http://YOUR_SERVER_IP:8080"
        }
    }
}
