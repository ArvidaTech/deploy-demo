def registry = "arvidatech/demo-pipeline"
def registryCredential = 'ArvidaDockerhub'
def IMAGE="${registry}:version-${env.BUILD_ID}"
def img =''
  
pipeline {
  agent any

  stages {
    stage("Build:Clone git repo") {
      steps {
        git 'https://github.com/ArvidaTech/build-demo.git'
      }
    }

    stage("Build: Maven Package") {
      steps {
        sh 'mvn package'
      }
    }
	
    stage("Build: docker img") {
      steps {
        img = docker.build("$IMAGE",  '.')
      }
    }
	
    stage("Build: push docker image") {
      steps {
	      docker.withRegistry('', ${registryCredential} ) {
			img.push 'latest'
		     img.push()
		}	
      }
    }
  }
}
