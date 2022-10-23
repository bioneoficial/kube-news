

 pipeline{
	agent any
	stages {
		stage ('Build Docker Image'){
			steps {
			script {
					dockerapp = docker.build("bioneoficial/kube-news:v1", '-f ./src/Dockerfile ./src')
				}
			}
		}
		
		stage ('Push Docker Image'){
			steps {
				script {
						docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
							dockerapp.push('v1')
							//dockerapp.push("${env.BUILD_ID}")
						}
					}
			}
		}
		stage ('Deploy Docker Image'){
			//environment{
				//tag_version = "${env.BUILD_ID}"
        		//MY_KUBECONFIG = credentials('kubeconfig')
			//}
			steps {
				script {
					//withKubeConfig(credentialsId: 'kubeconfig'){
					withKubeConfig([credentialsId: 'kubeconfig', serverUrl: 'https://40c34367-a3eb-4096-823b-9dc3ccf5fd29.k8s.ondigitalocean.com']){
						sh 'kubectl get pods'
						//sh("kubectl --kubeconfig $MY_KUBECONFIG get pods")
						//sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
						sh 'kubectl apply -f ./k8s/deployment.yaml'
					}
				}
			}
		}
	}
 }