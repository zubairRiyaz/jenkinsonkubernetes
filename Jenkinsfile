pipeline {
  agent {
    kubernetes {
      label 'nginximage'
      yaml """
apiVersion: v1
kind: Pod
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: jenkins-admin
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
