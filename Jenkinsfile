pipeline {
  environment {
    registry = "thukten/simplilearn"
    registryCredential = 'dockerhub'
  }
  agent any
  stages {
	stage('Cloning Git') {
	steps {
		git 'https://github.com/thukten/dockerizing-jenkins.git'
	      }
	}

        stage('Building image') {
        steps{
            script {
            dockerImage = docker.build registry + ":$BUILD_NUMBER"
            }
        }
        }

        stage('Deploy Image') {
        steps{
            script {
            docker.withRegistry( '', 'dockerhub' ) {
                dockerImage.push()
            }
            }
        }
        }

        stage('Remove Image') {
        steps{
            sh "docker rmi $registry:$BUILD_NUMBER"
        }
        }
   }   
}

node {
    stage('Execute Image'){
        def customImage = docker.build("thukten/dockerizing-jenkins-pipeline:${env.BUILD_NUMBER}")
        customImage.inside {
            sh 'echo This is the code executing inside the container.'
        }
    }
}
