pipeline {
  environment {
    dockerimagename = "sasodimovski/mongo"
    dockerImage = ""
	KUBECONFIG = "C:\\Users\\SASHO\\.kube\\config"
  }
  agent any
  stages {
    stage('Pull од GitHub') {
      steps {
        git 'https://github.com/SasoDimovski/mongo.git'
      }
    }
    stage('Build на Docker image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Push на DockerHub') {
      environment {
          registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploy на Kubernetes ') {
      steps {
          script {
		 bat 'kubectl apply -f mongo-config.yaml'
		 bat 'kubectl apply -f mongo-secret.yaml'
		 bat 'kubectl apply -f mongo.yaml'
		 bat 'kubectl apply -f webapp.yaml'
        }
      }
    }
  }
}