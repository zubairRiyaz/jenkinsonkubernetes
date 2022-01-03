pipeline {
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git url:'https://github.com/zubairriyaz/jenkinsonkubernetes.git', branch:'main'
      }
    }
    
    stage('Building image') {
      steps {
        sh 'docker build -t zubairbhat722/nginximage .'
        sh 'docker tag nginximage zubairbhat722/nginximage:$BUILD_NUMBER'
      }
    }
    
    stage('Deploy Image') {
      steps {
        withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
          sh  'docker push zubairbhat722/nginximage'
          sh  'docker push zubairbhat722/nginximage:$BUILD_NUMBER'
        }  
      }
    }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }
    
    stage('Remove Unused docker image') {
      steps {
        sh 'docker rmi $registry:$BUILD_NUMBER'
      }
    }
  }
}
