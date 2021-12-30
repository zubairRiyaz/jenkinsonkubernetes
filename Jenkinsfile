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
    stage('Deploy Image') {
      steps{
        script {
          withDockerRegistry([ credentialsId: "registryCredential", url: "https://index.docker.io/v1/" ]) {
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
