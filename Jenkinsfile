pipeline {
  agent {
    kubernetes {
      yaml '''
      apiVersion: v1
      kind: Pod
      metadata:
        labels:
          app: jenkins-k8s-deployment
      spec:
        containers:
        - name: kubectl
          image: bitnami/kubectl:latest # kubectl을 포함한 컨테이너 이미지
          command:
          - cat
          tty: true
          volumeMounts:
          - mountPath: /root/.kube/config
            name: kube-config
        volumes:
        - name: kube-config
          configMap:
            name: kubeconfig-configmap # Kubernetes에 설정된 kubeconfig
      '''
    }
  }
  stages {
    stage('git pull') {
      steps {
        git url: 'https://github.com/rednine9777/GitOps.git', branch: 'main'
      }
    }
    stage('k8s deploy') {
      steps {
        container('kubectl') {
          sh 'kubectl apply -f deployment.yaml' // 기존 deployment.yaml 파일 그대로 사용
        }
      }
    }    
  }
}
