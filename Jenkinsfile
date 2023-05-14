#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage('test') {
            steps {
                script {
                    echo "Testing the application...."
                }
            }
        }
        stage('build') {
            steps {
                script {
                    echo "Building the application..."
                }
            }
        }
        stage( 'deploy on ec2') {
			steps {
				script {
					//login docker
                    echo "Deploying..."
					withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
				    	sh "echo $PASS | docker login -u $USER --password-stdin"
					}
					def dockerCmd = 'docker run —p 8000:80 —d yuisofull/demo:react-nodejs-example-1.0'
					//ssh into ec2 machine and deploy docker image
					sshagent(['docker']) {
						sh "ssh -o StrictHostKeyChecking=no docker@34.125.1.202 ${dockerCmd}"
                    }
                }
			}
        }
    }
}
