pipeline {
    agent any

    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }

    environment {
        DOCKER_IMAGE = "sauraabh/starbucks"
        DOCKER_TAG = "latest"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Git Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/saurabhpaljhs-maker/Starbucks-Application.git'
            }
        }

        stage('Install NPM Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t starbucks .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh "docker tag starbucks ${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker') {
                        sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Docker Image pushed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
