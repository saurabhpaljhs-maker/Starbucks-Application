pipeline {
    agent any

    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }

    environment {
        IMAGE_NAME = "sauraabh/starbucks"
        IMAGE_TAG = "latest"
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

        stage('Verify Docker Access') {
            steps {
                sh '''
                whoami
                groups
                docker version
                docker ps
                '''
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
                sh "docker tag starbucks ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker') {
                        sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Docker Image Successfully Pushed to DockerHub'
        }
        failure {
            echo 'Pipeline Failed'
        }
        always {
            cleanWs()
        }
    }
}
