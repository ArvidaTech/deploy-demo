pipeline {
    environment {
	registry = "arvidatech/demo-pipeline"
	registryCredential = 'ArvidaDockerhub'
	IMAGE="${registry}:version-${env.BUILD_ID}"
    }
    
    /*def registry = "arvidatech/demo-pipeline"
    def registryCredential = 'ArvidaDockerhub'
    def IMAGE="${registry}:version-${env.BUILD_ID}"*/
    agent any
    stages{
	    stage('Build - Clone') {
		  steps{
			  git 'https://github.com/ArvidaTech/build-demo.git'
		  }
	    }
	    stage('Build - Maven package'){
		    steps{
		    	sh 'mvn package'
		    }
	    }
	    def img = stage('Build') {
		    steps{  
			    docker.build("$IMAGE",  '.')
		    }
	    }
	    stage('Build - Test') {
		    steps{
			    img.withRun("--name run-$BUILD_ID -p 8081:8080") { c ->
			    sh 'docker ps'
			    sh 'netstat -ntaup'
			    sh 'sleep 30s'
			    sh 'curl 127.0.0.1:8081'
			    sh 'docker ps'
		    	    }		
		    }
	    }
	    stage('Build - Push') {
		steps{

		  docker.withRegistry('', $registryCredential ) {
		      img.push 'latest'
		      img.push()
		  	}
		}
	    }
	    stage('Deploy - Clone') {
		steps{
		  git 'https://github.com/ArvidaTech/deploy-demo.git'
		}	    
	    }
	    
	    stage('Deploy - End') {
		steps{
		      ansiblePlaybook (
			  colorized: true,
			  become: true,
			  playbook: 'playbook.yml',
			 inventory: '${HOST},',
			  extras: "--extra-vars 'image==$IMAGE'"
		      )
		}	    
	    }
    }

}

