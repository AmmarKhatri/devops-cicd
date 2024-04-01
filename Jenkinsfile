pipeline {
    agent any

    environment {
        dockerImageName = "lones58/todo-app"
        kubeconfigCredentialId = "kubeconfig"
    }

    stages {
        stage('Checkout Source') {
             steps {
                // Checkout the source code from your Git repository
                git branch: 'main', url: ' https://github.com/AmmarKhatri/devops-cicd.git'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    dockerImage = docker.build dockerImageName
                }
            }
        }

        stage('Pushing Image') {
            environment {
                registryCredential = 'dockerhub-credentials'
            }
            steps {
                script {
                    //pushing to docker
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploying to Minikube') {
            steps {
                script {
                        //Applying kubeconfig
                        withCredentials([file(credentialsId: kubeconfigCredentialId, variable: 'KUBECONFIG')]) {
                        // Apply yml files
                        bat 'kubectl apply -f ./k8s/secrets.yml'
                        bat 'kubectl apply -f ./k8s/deployment.yml'
                    }
                }
            }
        }
    }
}