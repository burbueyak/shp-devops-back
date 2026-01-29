pipeline {
    agent any
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Starting Docker build..."
                    
                    if (!fileExists('Dockerfile')) {
                        error('Dockerfile not found in repository!')
                    }
                    
                    echo "=== Dockerfile content ==="
                    sh 'cat Dockerfile'
                    echo "========================="
                    
                    sh '''
                        # Собираем Docker образ с тегом, включающим номер сборки
                        docker build -t my-app:${BUILD_NUMBER} .
                        
                        # Проверяем, что образ успешно создан
                        echo "Docker images after build:"
                        docker images | grep my-app
                    '''
                    
                    echo "Docker image built successfully!"
                }
            }
            post {
                success {
                    echo "Image tag: my-app:${BUILD_NUMBER}"
                    archiveArtifacts artifacts: 'Dockerfile', fingerprint: true
                }
                failure {
                    echo "Docker build failed!"
                    echo "Check:"
                    echo "1. Is Docker installed and running?"
                    echo "2. Is Dockerfile present in repository?"
                    echo "3. Does Dockerfile have correct syntax?"
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
