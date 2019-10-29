pipeline {
	environment {
    registry = "kingsleyijomah/jenkins-test"
    appImageTag = "latest.app"
    webImageTag = "latest.web"
    registryCredential = 'dockerhub'
  }

  agent any
  
  stages {
    stage('Building image') {
      steps{
      	sh 'docker-compose build'
      }
    }

    stage('Tag images') {
    	steps {
    		sh 'docker tag app ' + registry + appImageTag
    		sh 'docker tag web ' + registry + webImageTag
    	}
    }

    stage('Push Images to Registry') {
    	steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            sh 'docker push ' + registry + appImageTag
            sh 'docker push ' + registry + webImageTag
          }
        }
      }
    }
  }
}