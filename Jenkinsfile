pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t java-jenkins-demo:${BUILD_NUMBER} .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run --rm java-jenkins-demo:${BUILD_NUMBER}'
            }
        }

    }
}