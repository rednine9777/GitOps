pipeline {
  agent any
  stages {
    stage('Git Pull') {
      steps {
        git url: 'https://github.com/rednine9777/GitOps.git', branch: 'main'
      }
    }
    stage('K8s Deploy') {
      steps {
        withKubeConfig([credentialsId: 'kubeconfig']) {
          sh 'kubectl apply -f deployment.yaml'
        }
      }
    }    
  }
}
