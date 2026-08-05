        stage('Docker Build') {
            steps {
                script {
                    echo "Build Docker Image"
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

    } // closes stages

} // closes pipeline
