pipeline {
    agent any
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
                withCredentials([string(credentialsId: 'docker_cred', variable: 'DOCKERHUB_PASSWORD')]) {
                    sh "echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin"
                    sh "docker tag my-node-app:1.0 ayushrudra/my-node-app:1.0"
                    sh "docker push ayushrudra/my-node-app:1.0"
                }
            }
        }
}
