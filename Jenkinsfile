pipeline {

  environment {
    registry = "10.138.0.3:5001/prasanth1290/flask"
    registry_mysql = "10.138.0.3:5001/prasanth1290/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/kmprasanth90/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
environment {     
    DOCKERHUB_CREDENTIALS= credentials('kmpdocker')     
} 
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()  
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }
   stage('Build mysql image') {
     steps{
       sh 'docker build -t "10.138.0.3:5001/prasanth1290/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "10.138.0.3:5001/prasanth1290/mysql:$BUILD_NUMBER"'
        }
      }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }

}
