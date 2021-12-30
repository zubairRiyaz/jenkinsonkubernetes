pipeline {
  environment {
    registry = "zubairbhat722/myapacheimage"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/zubairRiyaz/jenkinsonkubernetes.git'
      }
    }
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
          docker.withRegistry( '', registryCredential ) {
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
