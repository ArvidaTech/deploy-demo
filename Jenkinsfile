pipeline {
  environment { 
    registry = "arvidatech/demo-pipeline" 
    registryCredential = 'ArvidaDockerhub' 
    dockerImage = '' 
  }
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
	  	script { 
          dockerImage = docker.build registry + ":$BUILD_NUMBER" 
		}
      }
    }
	
    stage("Build: push docker image") {
      steps {
	    script { 
		  docker.withRegistry('', registryCredential) {
			dockerImage.push 'latest'
			dockerImage.push()
			}
		}	
      }
    }
  }
}
