pipeline {
  environment {
  
    dockerImage = ''
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git url:'https://github.com/zubairRiyaz/jenkinsonkubernetes.git', branch:'main'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build("zubairRiyaz/jenkinsonkubernetes:${env.BUILD_ID}")
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
