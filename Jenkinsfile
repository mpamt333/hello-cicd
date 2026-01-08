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
                    
                    // 1. CHỈ ĐỊNH TÊN IMAGE VÀ REPOSITORY ĐẦY ĐỦ
                    def fullImageRepoName = "vinhpn12/hello-cicd" // <-- Tên Repository CHUẨN (KHÔNG CÓ DẤU "/")
                    
                    // 2. TẠO TAG NAME CUỐI CÙNG (Kết hợp bằng dấu hai chấm)
                    // Kết quả: vinhpn12/hello-cicd:8abe16f
                    def finalTagName = "${fullImageRepoName}:${commitSha}" 

                    echo "Building image with Tag: ${finalTagName}"

                    // 3. THỰC HIỆN BUILD (SỬ DỤNG DẤU NHÁY KÉP)
                    def builtImage = docker.build "${finalTagName}", '.'

                    // PUSH IMAGE
                    docker.withRegistry('https://docker.io', 'docker-hub-credentials-id') {
                        builtImage.push()
                    }
                    echo "Successfully pushed image: ${finalTagName} to Docker Hub."
                }
            }
        }
    }
}
