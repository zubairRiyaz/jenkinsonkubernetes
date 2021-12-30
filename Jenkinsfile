pipeline {
  environment {
    DOCKERHUB_CREDENTIALS = credentials(‘dockerhub’)
  }
  agent any
  stages {
    stage('Building image') {
      steps {
        sh ‘ sudo docker build -t zubairbhat722/nginximage  .’
      }
    }
    stage('login') {
      steps {
        sh ‘echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR —password-stdin’
      }
    }  
    
    stage('Deploy Image') {
      steps {
        sh 'docker push zubairbhat722/nginximage'
        
      }
    }
    stage('Remove Unused docker image') {
      steps {
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
  Post {
    Always {
        sh ‘docker logout’
    }
  }
}
