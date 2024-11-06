pipeline {
  agent {
    kubernetes {
      yaml '''
      apiVersion: v1
      kind: Pod
      metadata:
        labels:
          app: blue-green-deploy
        name: blue-green-deploy
      spec:
        containers:
        - name: kustomize
          image: sysnet4admin/kustomize:3.6.1
          tty: true
          volumeMounts:
          - mountPath: /usr/local/bin/kubectl
            name: kubectl
          command:
          - cat
        serviceAccount: jenkins
        volumes:
        - name: kubectl
          hostPath:
            path: /usr/local/bin/kubectl
      '''
    }
  }
  stages {
    stage('Git Pull') {
      steps {
        git url: 'https://github.com/rednine9777/GitOps.git', branch: 'main'
      }
    }
    stage('Check Files') {
      steps {
        container('kustomize') {
          sh 'ls -al' // 작업 디렉토리 내 파일 확인
        }
      }
    }
    stage('K8s Deploy') {
      steps {
        container('kustomize') {
          withKubeConfig([credentialsId: 'kubeconfig-secret-text']) {
            sh 'kubectl apply -f deployment.yaml' // 경로 확인 후 배포
          }
        }
      }
    }    
  }
}
