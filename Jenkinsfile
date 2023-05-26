#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master',retriever: modernSCM(//call the library
    [$class : 'GitSCMSource',
    remote: 'https://github.com/yuisofull/jenkins-shared-library.git',
    credentialsID: 'git-repo'
    ]
)

pipeline {
    agent any
    tools {
        maven 'maven-3.6'
    }
    environment {
        IMAGE_NAME = 'yuisofull/demo:jvm-sshagent'
    }
    stages{
        stage('build app') {
            steps {
                script {
                    echo "Building the application..."
                    buildJar()
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    echo "Building the image..."
                    dockerLogin() 
                    buildImage(env.IMAGE_NAME)
                    dockerPush(env.IMAGE_NAME)
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
                    echo "Deploying docker image to EC2..."

                    def ggInstance = "docker@34.125.125.162"
                    def dockerComposeCmd = "docker-compose -f docker-compose.yaml up --detach"

                    sshagent(['docker']) {
                        sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ${ggInstance}:/home/docker"
                        sh "shh -o StrictHostKeyChecking=no ${ggInstance} ${dockerComposeCmd}"
                    }
                }
            }
        }
    }
}
