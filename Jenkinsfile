def registry = "arvidatech/demo-pipeline"
def registryCredential = 'ArvidaDockerhub'
def IMAGE="${registry}:version-${env.BUILD_ID}"
  
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
	
    def img = stage("Build: docker img") {
      steps {
        docker.build("$IMAGE",  '.')
      }
    }
	
    stage("Build: push docker image") {
      steps {
        docker.withRegistry('', $registryCredential ) {
			img.push 'latest'
		     img.push()
		}	
      }
    }
  }
}
