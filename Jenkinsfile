pipeline {
    agent any
    environment {
        DOCKERHUB_USERNAME = credentials('DOCKERHUB_USERNAME')
        DOCKERHUB_PASSWORD = credentials('DOCKERHUB_PASSWORD')
    }
    stages {
        stage("checkout") {
            steps {
                checkout scm
            }
        }

        stage("Test") {
            steps {
                sh 'sudo npm install'
                sh 'npm test'
            }
        }

        stage("Build") {
            steps {
                sh 'npm run build'
            }
        }

        stage("Build Docker-Image") {
            steps {
                sh 'docker build -t my-node-app:1.0 .'
            }
        }

        stage('Push-Docker-Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'DOCKERHUB_CREDENTIALS_ID', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        sh "docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD"
                        sh "docker tag my-node-app:1.0 ayushrudra/my-node-app:1.0"
                        sh "docker push ayushrudra/my-node-app:1.0"
                    }
                }
            }
        }
    }
}
