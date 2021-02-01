pipeline {
    environment { 
        registry = "niranjankolli/helloapp"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning our Git') {
            steps {
                git branch: 'main', credentialsId: 'gitlab', url: 'https://github.com/Niranjankolli/Jenkins-K8s-Integration.git'
            }
        } 
        stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build("niranjankolli/helloapp:${env.BUILD_ID}") 
                }
            } 
        }
        stage('Deploy our image') { 
            steps { 
                script { 
                    docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) { 
                        dockerImage.push("latest")
                        dockerImage.push("${env.BUILD_ID}") 
                    }
                } 
            }
        }
        stage('Deploy app to K8s') {
            steps {
                script {                    
                    withKubeConfig([credentialsId: 'kubeconfig', serverUrl: 'https://10.128.130.84:6443']) {
                        sh 'kubectl apply -f vaas.yaml'
                    }
                }                
            }
        }
    }
}
