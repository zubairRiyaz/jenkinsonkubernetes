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
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: dockersock
  volumes:
    - name: dockersock
      hostPath:
        path: /var/run/docker.sock
    - name: jenkins-data
      persistentVolumeClaim:
          claimName: jenkins
    
"""
              }
   }
  parameters {
    string(name: 'version', description: 'App version to deploy')
    choice(
        name: 'myParameter',
        choices: ['dev', 'prod'],
        description: 'Environment where the app should be deployed' )
  }
  stages {
    stage('Building image') {
      steps {
        container('docker') {
          
           sh 'docker build -t zubairbhat722/nginximage:latest .'
        }
      }
    }
    stage('Publish image to Docker Hub') {
          
            steps {
              container('docker') {
        withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
          sh  'docker push zubairbhat722/nginximage:latest'
           
              }
        }
                  
          }
    }
    stage('Deploy') {
      // Deploy app version ${params.version} to ${params.env} env
     
      //add release information to the dashboard
      steps {
        addDeployToDashboard(
          env: params.env,
          buildNumber: params.version
        )
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
