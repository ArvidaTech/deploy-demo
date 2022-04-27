pipeline {
  environment { 
    registry = "arvidatech/demo-pipeline" 
    registryCredential = 'ArvidaTech' 
    dockerImage = '' 
  }
  agent any
  stages { 
	    	 	  
    stage("Pre-Deploy:Clone git repo") {
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
		  extras: "--extra-vars 'image=${registry}:${env.BUILD_ID}'"
		)
	  }	
    }
    stage("Jira ADD Comment") {
      steps {
        jiraComment body: '[SUCCESS]The application is deployed', issueKey: 'FIRST-2'
      }
    }
	
  }
}
