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
    			sshagent(['ssh-key']) {
    		
    				sh 'scp -o StrickHostKeyChecking=yes Dockerfile ec2-user@3.235.94.113:/home/ec2-user'
    				sh 'scp -o StrickHostKeyChecking=yes docker-image.yaml ec2-user@3.235.94.113:/home/ec2-user'
    			}
    		}
		}

		stage('build the image') {
			steps {
				sshagent(['ssh-key']) {
					sh 'ssh -o StrickHostKeyChecking=yes ec2-user@3.235.94.113 \"ansible-playbook docker-image.yaml\"'

				}

			}
		}
	}
}
