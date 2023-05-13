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
        stage("commit version update") {
            steps {
                script {
                    echo "commiting the updated version..."
    			    withCredentials([usernamePassword(credentialsId: 'git-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        sh 'git status'
                        sh 'git branch'
                        sh 'git config --list'

                        sh "git remote set-url origin https://${USER}:${PASS}@github.com/yuisofull/jma1.git"
                        sh 'git add . && git commit -m "ci:version bump" && git push'
                	}
            	}
           }
	    }
    }   
}
