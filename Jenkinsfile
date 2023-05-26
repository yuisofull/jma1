#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        maven 'maven-3.6'
    }
    stages {
        stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'//find in pom.xml with the pattern
                    def version = matcher[0][1]//match the pattern with content between tags
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }
        stage('build app') {
            steps {
                script {
                    echo "Building the application..."
                    sh "mvn clean package"
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    echo "Building the image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
        			sh "echo $PASS | docker login -u $USER --password-stdin"
        			sh "docker build -t yuisofull/demo:${IMAGE_NAME} ."
        			sh "docker push yuisofull/demo:${IMAGE_NAME}"
                    }
                }
            }
        }
        stage('test') {
            steps {
                script {
                    echo "Testing the application..."
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo "Deploying the application..."
                }
            }
        }
        stage("commit version update") {
            steps {
                script {
                    echo "commiting the updated version..."
    		        withCredentials([usernamePassword(credentialsId: 'github-api', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        //sh 'git config --global user.email "jenkins@example.com"'
                        //sh 'git config --global user.name "jenkins"'

                        sh 'git status'
                        sh 'git branch'
                        sh 'git config --list'

                        sh "git remote set-url origin https://${PASS}@github.com/${USER}/jma1.git"
                        sh 'git add . && git commit -m "ci:version bump" && git push origin HEAD:jenkins-jobs'
                	}
            	}
            }
	}
    }
}
