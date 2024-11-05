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
        container('kustomize'){
            withKubeConfig([credentialsId: 'kubeconfig']) {
            sh 'kubectl apply -f deployment.yaml'
          }
        }
      }
    }    
  }
}
