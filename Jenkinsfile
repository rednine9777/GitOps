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
    stage('K8s Deploy') {
      steps {
        withKubeConfig([credentialsId: 'kubeconfig-secret-text']) { // Secret Text ID 참조
          sh 'kubectl apply -f *.yaml' // kubectl 명령어로 모든 yaml 파일을 배포
        }
      }
    }    
  }
}
