pipeline {
	agent any
	tools {
		maven 'maven'
	}
	stages {
		stage('git checkout') {
			steps {
				checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CleanCheckout']], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/anuragnalapur/pipeline.git']]])
			}
		}

		stage('maven build') {
			steps {
				
				sh 'mvn clean package'
				
			}
		}
				
		
    		stage('copying dockerfile and playbook to ansible server') {
    			steps {
    			
    		
    				sh 'scp -o Dockerfile ec2-user@3.235.94.113:/home/ec2-user'
    				sh 'scp -o docker-image.yaml ec2-user@3.235.94.113:/home/ec2-user'
    			
    			}
		}

		stage('build the image') {
			steps {
				
					sh 'ssh -o StrickHostKeyChecking=no ec2-user@3.235.94.113 \"ansible-playbook docker-image.yaml\"'

				

			}
		}
	}
}
