#!/usr/bin/env groovy

pipeline {
    agent none
    tools {
        maven 'maven-3.6'
    }
    stages {
        stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                    -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} versions:commit'
                    def matcher = readfile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
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
    }
}
