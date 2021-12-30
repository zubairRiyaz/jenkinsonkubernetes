pipeline {
  environment {
    registry = "zubairbhat722/nginximage"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build("zubairriyaz/jenkinsonkubernetes:${env.BUILD_ID}")
        }
      }
    }
    stage ('login') {
      steps{
        sh ‘echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR —password-stdin’
      }
    }  
    stage('Deploy Image') {
      steps{
        script {
          withDockerRegistry([ registryCredential: "dockerhub", url: "https://registry.hub.docker.com" ]) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
