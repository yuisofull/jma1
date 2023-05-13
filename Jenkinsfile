#!/usr/bin/env groovy
def gv

pipeline {
    agent any
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    	echo "building the docker image..."
    			withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
        			sh "echo $PASS | docker login -u $USER --password-stdin"
        			sh 'docker build -t yuisofull/demo:jma-1.1 .'
        			sh 'docker push yuisofull/demo:jma-1.1'
                    	//gv.buildJar()
                	}
            	}
           }
	}
        stage("build image") {
            steps {
                script {
                    echo "building image"
                    //gv.buildImage()
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    echo "deploying"
                    //gv.deployApp()
                }
            }
        }
    }   
}
