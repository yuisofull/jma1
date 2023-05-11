#!/usr/bin/env groovy

@Library('jenkins-shared-library')//call the library

def gv

pipeline {
    agent any
    tools {
        maven 'maven-3.6'
    }
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
                    buildJar() //use the library
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    buildImage 'yuisofull/demo:jma-1.3' //pass the parameter to the library
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }   
}
