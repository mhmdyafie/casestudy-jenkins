pipeline {
    agent any

    environment {
        IMAGE = 'https://github.com/mhmdyafie/casestudy-jenkins.git'
        TAG = 'latest'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def app = docker.build("${IMAGE}:${TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-creds') {
                        docker.image("${IMAGE}:${TAG}").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes (Helm)') {
            steps {
                script {
                    sh """
                    helm upgrade --install myapp helm/myapp \
                    --set image.repository=${IMAGE} \
                    --set image.tag=${TAG}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline berhasil"
        }
        failure {
            echo "❌ Pipeline gagal, cek log di Console Output"
        }
    }
}
