pipeline {

  environment {
    registry = "prasanth1290/flask"
    registry_mysql = "prasanth1290/mysql"
    DOCKERHUB_CREDENTIALS= credentials('kmpdocker')	  
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

 stage('Login to Docker Hub') {      	
     steps{                       	
	sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'                		
 echo 'Login Completed'      
     }           
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
       sh 'docker build -t "prasanth1290/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "prasanth1290/mysql:$BUILD_NUMBER"'
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
