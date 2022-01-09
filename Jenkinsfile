pipeline {
  agent {
    kubernetes {
      label 'zubairbhat722/nginximage'
      defaultContainer 'jnlp'
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
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: dockersock
  volumes:
    - name: jenkins-data
      persistentVolumeClaim:
          claimName: jenkins-pv-claim
    - name: dockersock
      hostPath:
        path: /var/run/docker.sock
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
