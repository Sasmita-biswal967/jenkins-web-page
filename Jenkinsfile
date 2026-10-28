
pipeline {
    agent any

    options {
        timestamps()
    }

    environment {
        DOCKERHUB_USER = 'sasmita205'
        IMAGE_NAME = 'github-jenkins-web'
        CONTAINER_NAME = 'github-jenkins-web'
    }

    stages {
        stage('Checkout GitHub') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                      -t "$DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER" \
                      -t "$DOCKERHUB_USER/$IMAGE_NAME:latest" .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DH_USER',
                        passwordVariable: 'DH_TOKEN'
                    )
                ]) {
                    sh '''
                        set +x
                        echo "$DH_TOKEN" | docker login \
                          -u "$DH_USER" --password-stdin

                        docker push "$DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER"
                        docker push "$DOCKERHUB_USER/$IMAGE_NAME:latest"

                        docker logout
                    '''
                }
            }
        }

        stage('Deploy Website') {
            steps {
                sh '''
                    docker pull "$DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER"

                    docker rm -f "$CONTAINER_NAME" || true

                    docker run -d \
                      --name "$CONTAINER_NAME" \
                      --restart unless-stopped \
                      -p 80:80 \
                      "$DOCKERHUB_USER/$IMAGE_NAME:$BUILD_NUMBER"
                '''
            }
        }
    }

    post {
        success {
            echo 'Website deployed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
