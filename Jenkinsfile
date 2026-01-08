pipeline {
    agent any

    environment {
        IMAGE_REPO = "vinhpn12/hello-cicd"
    }

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-credential-id',
                    url: 'https://github.com/mpamt333/hello-cicd.git'
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                script {
                    def commitSha = sh(
                        script: 'git rev-parse --short HEAD',
                        returnStdout: true
                    ).trim()

                    def imageTag = "${IMAGE_REPO}:${commitSha}"
                    echo "Building Docker image: ${imageTag}"

                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-credentials-id',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        sh """
                          docker build -t ${imageTag} .
                          echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                          docker push ${imageTag}
                        """
                    }

                    echo "Successfully pushed image: ${imageTag}"
                }
            }
        }
    }
}
