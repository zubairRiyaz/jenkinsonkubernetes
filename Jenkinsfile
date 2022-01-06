pipeline {
  agent{ label 'jenkins-jenkins-agent' }
  stages {
    stage('Checkout Source') {
      steps {
        git url:'https://github.com/zubairriyaz/jenkinsonkubernetes.git', branch:'main'
      }
    }
    
    stage('Building image') {
      steps {
        sh 'docker build -t zubairbhat722/nginximage .'
      }
    }
    
    stage('Deploy Image') {
      steps {
        withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
          sh  'docker push zubairbhat722/nginximage'
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
