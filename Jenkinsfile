pipeline {
  environment { 
    registry = "arvidatech/demo-pipeline" 
    registryCredential = 'ArvidaTech' 
    dockerImage = '' 
  }
  agent any
  stages { 
	  
    stage("Build:Clone git repo") {
      steps {
        git branch: 'main', url: 'https://github.com/ArvidaTech/build-demo.git'
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
    stage("Deploy:Clone git repo") {
      steps {
        git branch: 'main', url: 'https://github.com/ArvidaTech/deploy-demo.git'
      }
    }
    stage("Deploy - With Ansible") {
      steps{
	    ansiblePlaybook (
		  colorized: true,
		  become: true,
		  credentialsId: 'private_key',
		  playbook: 'playbook.yml',
		  inventory: '${HOST},',
		  extras: "--extra-vars 'image=dockerImage'"
		)
	  }	
    }
	
  }
}
