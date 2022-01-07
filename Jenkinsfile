pipeline {
  agent 
    kubernetes {
      label 'jenkins-jenkins-agent'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: jenkins-sc
  containers:
  - name: nginximage
    image: zubairbhat722/nginximage
    command:
    - cat
    tty: true
    volumeMounts:
      - mountPath: /var/jenkins_home
        name: jenkins-home
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
   - name: jenkins-home
      persistentVolumeClaim:
        claimName: jenkins-pvc
"""
}
   }
  
  stages {
    stage('Checkout Source') {
      steps {
        git url:'https://github.com/zubairriyaz/jenkinsonkubernetes.git', branch:'main'
        
      }  
    }
    stage('Building image') {
      steps {
        container('docker') {
          
           sh 'docker build -t jenkins-jenkins-agent:$BUILD_NUMBER .'
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
