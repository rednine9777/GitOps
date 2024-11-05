pipeline {
  agent any
  stages {
    stage('git pull') {
      steps {
        git url: 'https://github.com/rednine9777/GitOps.git', branch: 'main'
      }
    }
    stage('k8s deploy') {
      agent { label 'master' } // Jenkins에서 마스터 노드를 대상으로 실행할 경우 사용
      steps {
        kubernetesDeploy(kubeconfigId: 'kubeconfig', configs: '*.yaml')
      }
    }    
  }
}
