pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello, World!'
                sh '''
                   mkdir pavan
                   cd pavan
                   yum install httpd -y
                   
                   '''
            }
        }
    }
}
