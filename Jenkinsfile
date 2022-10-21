pipeline{
    agent any

    stages {
        stage ('Build Docker Image'){
            steps{
                script{
                    dockerapp = docker.build("jbatisjr/kube-news:${env.BUILD_ID}", '-f ./scr/Dcokerfile ./scr')
                }
            }
        }

        stage ('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

         stage ('Deploy Kubernetes'){
            steps{
                withKubeConfig([credentialId: 'Kubeconfig']){
                sh 'Kubectl apply -f ./k8s/deployment.yaml'
                }
            }
        }        
    }
}