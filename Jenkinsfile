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
        withKubeConfig([credentialsId: 'kubeconfig-secret-text']) { // Secret Text ID 참조
          sh 'kubectl apply -f *.yaml' // kubectl 명령어로 모든 yaml 파일을 배포
        }
      }
    }    
  }
}
