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
						def dockerLogin = "docker login -u ${USER2} -p ${PASS2}"
							def dockerCmd = "docker run -dp 80:8000 yuisofull/demo:react-nodejs-example-1.0"
							sh "sshpass -p ${PASS} ssh -T -o StrictHostKeyChecking=no docker@34.125.1.202 ${dockerLogin}"
							sh "sshpass -p ${PASS} ssh -T -o StrictHostKeyChecking=no docker@34.125.1.202 ${dockerCmd}"
						}
					}
                }
			}
        }
    }
}
