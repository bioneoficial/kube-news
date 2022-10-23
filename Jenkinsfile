

 pipeline{
	agent any
	stages {
		stage ('Build Docker Image'){
			steps {
			script {
					dockerapp = docker.build("bioneoficial/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
				}
			}
		}
		
		stage ('Push Docker Image'){
			steps {
				script {
						docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
							dockerapp.push('latest')
							dockerapp.push("${env.BUILD_ID}")
						}
					}
			}
		}
		stage ('Deploy Docker Image'){
			environment{
				tag_version = "${env.BUILD_ID}"
        		//MY_KUBECONFIG = credentials('kubeconfig')
			}
			steps {
				//sh("kubectl --kubeconfig $MY_KUBECONFIG get pods")
				script {
					withKubeConfig(credentialsId: 'kubeconfig'){
						sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
						sh 'kubectl apply -f ./k8s/deployment.yaml'
					}
				}
			}
		}
	}
 }