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
                    docker.withRegistry('https://registry.hub.docker.com/', 'dockerhub'){
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }

                }
            }
        }
        stage('Deploy Kubernetes'){
            environment{
                tag_version = "${env.BUILD_ID}"
            }
            steps{
                script{
                    withKubeConfig([credentialsId: 'kube_config']){
                        sh 'sed -i "s/{{TAG}}/$tag_version/g" ./deployment.yaml'
                        sh 'kubectl apply -f ./deployment.yaml'
                    }
                }
            }
        }
    }
}