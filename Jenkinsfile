pipeline {

  environment {
    registry = "uni-shpark/myweb"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        echo "Checkout Source START"
        git 'https://github.com/uni-shpark/cicdtest.git'
        echo "Checkout Source END"
      }
    }

    stage('Build image') {
      steps{
        script {
          echo "Build image START $BUILD_NUMBER"
          dockerImage = docker.build("testweb:$BUILD_NUMBER")
          echo "Build image END"
        }
      }
    }

    stage('Push Image') {
      environment {
               registryCredential = 'dockerhub'
           }
      steps{
        script {
          echo "Push Image START"
          docker.withRegistry( "https://docker.io/suhyung007", registryCredential ) {
            dockerImage.push()
          }
          echo "Push Image END"
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          echo "Deploy App START"
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
          echo "Deploy App END"
        }
      }
    }

  }

}
