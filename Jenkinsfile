pipeline {
  agent {
    kubernetes {
      label 'jenkins-jenkins-agent'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
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
        path: /var/run/docker.sock
    - name: jenkins-pv
      persistentVolumeClaim:
        claimName: jenkins-pvc
"""
}
   }
  
  stages {
    stage('Build') {
      steps {
        container('maven') {
          sh """
                        mvn package -DskipTests
                                                """
        }
      }
    }
    stage('Checkout Source') {
      steps {
        git url:'https://github.com/zubairriyaz/jenkinsonkubernetes.git', branch:'main'
        
      }  
    }
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
