pipeline {
    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mpamt333/hello-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t hello-cicd:latest .'
            }
        }
    }
}
