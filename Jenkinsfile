pipeline {
    environment { 
        registry = "niranjankolli/helloapp"
        registryCredential = 'DockerHub'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning our Git') {
            steps {
                git url:'https://github.com/Niranjankolli/Jenkins-K8s-Integration.git', branch:'main'
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
                    kubernetesDeploy(configs: "helloapp.yaml", kubeconfigId: "mykubeconfig")
                    }
                }                
            }
        }
    }
