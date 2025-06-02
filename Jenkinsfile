pipeline {
    agent any

    environment {
        IMAGE = 'yourdockerhubusername/yourimage'
        TAG = 'latest'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def image = docker.build("${IMAGE}:${TAG}")
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

        stage('Deploy to Kubernetes') {
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
            echo '✅ Pipeline sukses!'
        }
        failure {
            echo '❌ Pipeline gagal. Cek log Console Output.'
        }
    }
}
