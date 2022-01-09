pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
          labels:
            label: mylabel
spec:
  # Use service account that can deploy to all namespaces
  containers:
 
  - name: docker
    image: docker:latest
    command:
    - cat
    tty: true
"""
              }
   }
  
  stages {
    stage('Building image') {
      steps {
        container('docker') {
          
           sh 'docker build -t zubairbhat722/nginximage:latest .'
        }
      }
    }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "nginx.yaml", kubeconfigId: "mykubeconfigfile")
        }
      }
    }
    
  }
}
