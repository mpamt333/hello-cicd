pipeline {
    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main',
                    credentialsId: 'token-master-pn12', // Dùng PAT cho GitHub
                    url: 'https://github.com/mpamt333/hello-cicd.git'
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                script {
                    // Lấy mã Commit ID làm Tag
                    def commitSha = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    def tagName = "myapp:${commitSha}"
                    
                    echo "Building image with Tag: ${tagName}"
                    
                    // 1. BUILD IMAGE (Cú pháp đúng)
                    def builtImage = docker.build tagName, '.' 
                    
                    // 2. PUSH IMAGE
                    // Thay 'docker-hub-credentials-id' bằng ID Credentials Docker Hub của bạn trong Jenkins
                    docker.withRegistry('https://docker.io', 'docker-hub-credentials-id') { 
                        builtImage.push()
                    }
                    echo "Successfully pushed image: ${tagName} to Registry."
                }
            }
        }
    }
}
