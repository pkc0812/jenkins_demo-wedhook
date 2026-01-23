pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello, World!'
                sh '''
                    docker build -t pkc08122 .
                    docker run -it -d -p 8080:80 --name pkc-cn pkc08122
                    docker ps 
                    docker ps -a
                '''
            }
        }
    }
}
