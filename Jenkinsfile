pipeline {
    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main',
		    credentialsId: 'bf91c801-918d-4864-b770-ecc1d59f8cd6',
                    url: 'https://github.com/mpamt333/hello-cicd.git'
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                script {
                    def commitSha = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    def tagName = "myapp:${commitSha}"
                    
                    echo "Building image with Tag: ${tagName}"
                    
                    def builtImage = docker.build tagName, '.' 
                    
                    docker.withRegistry('https://docker.io', 'docker-hub-credentials-id') { 
                        builtImage.push()
                    }
                    echo "Successfully pushed image: ${tagName} to Registry."
                }
            }
        }
    }
}
