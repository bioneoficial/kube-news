

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
							dockerapp.push('v1')
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
				script {
					withKubeConfig(credentialsId: 'kubeconfig'){
					// withKubeConfig([credentialsId: 'kubeconfig', caCertificate: 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURKekNDQWcrZ0F3SUJBZ0lDQm5Vd0RRWUpLb1pJaHZjTkFRRUxCUUF3TXpFVk1CTUdBMVVFQ2hNTVJHbG4KYVhSaGJFOWpaV0Z1TVJvd0dBWURWUVFERXhGck9ITmhZWE1nUTJ4MWMzUmxjaUJEUVRBZUZ3MHlNakV3TWpNeApPVFF4TkRkYUZ3MDBNakV3TWpNeE9UUXhORGRhTURNeEZUQVRCZ05WQkFvVERFUnBaMmwwWVd4UFkyVmhiakVhCk1CZ0dBMVVFQXhNUmF6aHpZV0Z6SUVOc2RYTjBaWElnUTBFd2dnRWlNQTBHQ1NxR1NJYjNEUUVCQVFVQUE0SUIKRHdBd2dnRUtBb0lCQVFESzVuYUVER0F0a2xHcm9xRDlwWmxIRWhnS0dtbDJYWlhQbC8wSkIwcWlzcGxTR2JhMwo3MzZkVFpKKzhCTXRjZXI4VFBkZHQveGd3ZERpU2dMdzJoZENKZi9CZ1lCWXhGRnhDRGFIWWIxVUdBVTcvVG0vCnNsWW83VDhNY1QwV2VoRGpkRFRkMjdaa05ORDBsVG92VFJwZ01xcldjQkYyUGJOVWZTZDBGcXNkRFU3MGY4TGIKdG9DOThFWVZrZnVXU0t1VTEvWXNRTTlRMS80MFFnMVNVeXA3cXpITnBsTytRZnpKWk95Y0tIMzNjTXdCVUh4cApabFB3d1ZwcWtBaDlVRllkdlVEcEtvdWxFZGRFa3BjNjhMQjVvODJHQkZsUGdJazJnY0w4L05Hc3hva0NTdFBtClVKYlZQb0tFTzZoODJMYXN4Yzl3QWZ5eUN1Q3lhVENUQW1DbkFnTUJBQUdqUlRCRE1BNEdBMVVkRHdFQi93UUUKQXdJQmhqQVNCZ05WSFJNQkFmOEVDREFHQVFIL0FnRUFNQjBHQTFVZERnUVdCQlM3aXVEbzdyUlR2bWdXeHZTMwp4UTVsb2w4UXV6QU5CZ2txaGtpRzl3MEJBUXNGQUFPQ0FRRUFxQW11N3UwNUNVWjh3OFhEV0NLUzNob05XdCtSCkl6RHVZQnpwSjFTMnZsbkNlOXJaUmFFYzdRaUlVNzViK3FWVFZ4MzhGaTZtZkN1YkFxNTRsZUU2U1dKejZTRzkKS0tJMGVaK1dmaUNMK0Q0R3BtQXdObFV2bTM1czRDc3ZGb1NZMGxZWEJINldXUUlHbnE5U3RTdzhLUnVKdDN6TAptcVdna3VWTlpSQmlYK0lIMmU0WTBpTmNKbTdIRGgzYkpIRDk1SmlMbzdxWnVuSGVzQ0czRUcrdmJ0ZlhhQXJSCmEyWEhNUFMwSXhWYkpQVUR2T1krV1FtWEEyVExtNlZkUHpGRnN3THVDNTltVUdpV2VBY1oxZnI0QWxTVlNTR2UKcGEzZlBLVDVvTE9mM1NrcnNONkE4ZFRVY2p1M3g4bnZoNzhmS0R5TC8vejNBVHE3TzFSSldKclRLUT09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K',
					//  serverUrl: 'https://40c34367-a3eb-4096-823b-9dc3ccf5fd29.k8s.ondigitalocean.com',contextName: 'do-nyc1-k8s',
                    // clusterName: 'do-nyc1-k8s',
                    // namespace: 'do-nyc1-k8s']){
						sh 'kubectl get pods'
						//sh("kubectl --kubeconfig $MY_KUBECONFIG get pods")
						sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
						sh 'kubectl apply -f ./k8s/deployment.yaml'
					}
				}
			}
		}
	}
 }