pipeline {
  agent any
  stages {
    stage('git pull') {
      steps {
        git url: 'https://github.com/rednine9777/GitOps.git', branch: 'main'
      }
    }
    stage('k8s deploy'){
      steps {
        sh 'kubectl apply -f deployment.yaml'
      }
    }    
  }
}
