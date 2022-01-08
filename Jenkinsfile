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
  serviceAccountName: jenkins
  containers:
 
  - name: docker
    image: docker:latest
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.soc
      persistentVolumeClaim:
        claimName: jenkins-pvc
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
