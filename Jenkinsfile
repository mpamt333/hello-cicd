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
		script {
			def commitSha = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
			def tagName = "myapp:${commitSha}"
			echo "Building image with Tag: ${tagName}"
			docker.build -t tagName .
			#docker.withRegistry('https://', 'docker-credentials-id') {
			#docker.image(tagName).push()
		}
			#sh 'docker build -t hello-cicd:latest .'
            }
        }
    }
}
