pipeline{
    agent any

    stages{
        stage('Build Docker Image'){
            steps{
                script{
                    dockerapp = docker.build("bioneoficial/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
        stage('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('https://hub.docker.com', 'dockerhub')
                    dockerapp.push('v1')
                    dockerapp.push("${env.BUILD_ID}")
                }
            }
        }
    }
}