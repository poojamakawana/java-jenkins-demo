pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'YOUR_DOCKER_USERNAME/java-jenkins-demo'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                    -t ${DOCKER_IMAGE}:${BUILD_NUMBER} \
                    -t ${DOCKER_IMAGE}:latest \
                    .
                '''
            }
        }
        stage('Run Container') {
                    steps {
                        sh '''
                            docker run --rm ${DOCKER_IMAGE}:${BUILD_NUMBER}
                        '''
                    }
                }
                stage('Push to Docker Hub') {
                    steps {
                        withCredentials([
                            usernamePassword(
                                credentialsId: 'dockerhub-credentials',
                                usernameVariable: 'DOCKER_USERNAME',
                                passwordVariable: 'DOCKER_PASSWORD'
                            )
                        ]) {
                            sh '''
                                echo "$DOCKER_PASSWORD" | docker login \
                                -u "$DOCKER_USERNAME" \
                                --password-stdin

                                docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                                docker push ${DOCKER_IMAGE}:latest

                                docker logout
                            '''
                        }
                    }
                }
        }
    }