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
					withCredentials([usernamePassword(credentialsId: 'docker-gcp-ssh', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
				    	withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS2', usernameVariable: 'USER2')]) {
							def dockerLogin = "echo ${PASS2} | docker login -u ${USER2} --password-stdin"
							def dockerCmd = "docker run -dp 8000:80 yuisofull/demo:react-nodejs-example-1.0"
							sh "sshpass -p ${PASS} ssh -o StrictHostKeyChecking=no docker@34.125.100.56 ${dockerLogin} && ${dockerCmd}"
						}
					}
                }
			}
        }
    }
}
