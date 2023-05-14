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
					sh 'sudo apt-get update'
                	sh 'sudo apt-get install -y sshpass'
					withCredentials([usernamePassword(credentialsId: 'docker-gcp-ssh', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
				    	withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS2', usernameVariable: 'USER2')]) {
							def dockerLogin = "echo ${PASS2} | docker login -u ${USER2} --password-stdin"
							def dockerCmd = 'docker run —p 8000:80 —d yuisofull/demo:react-nodejs-example-1.0'
							sh "ssh -p ${PASS} ssh docker@34.125.100.56 ${dockerLogin}"
							sh "ssh -p ${PASS} ssh docker@34.125.100.56 ${dockerCmd}"
						}
					}
                }
			}
        }
    }
}
