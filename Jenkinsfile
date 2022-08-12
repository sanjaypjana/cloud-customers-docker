pipeline {

  environment {
    dockerimagename = "sanjayjanarthanan/dotnet6webapi"
  }
  agent any

  stages {

    stage('Checkout stage') {
      steps {
        git 'https://github.com/sanjaypjana/dotnetwebapi'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'pulldockerlogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "KubeManifests", kubeconfigId: "kubeconfig")
        }
      }
    }

  }

}