#!/usr/bin/env groovy

//@Library('jenkins-shared-library')//call the library if you have define it in Global Pipeline Libraries

library identifier: 'jenkins-shared-library@master',retriever: modernSCM(//call the library
    [$class : 'GitSCMSource',
    remote: 'https://github.com/yuisofull/jenkins-shared-library.git',
    credentialsID: 'git-repo'
    ]
)

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
                    dockerLogin()
                    buildImage 'yuisofull/demo:jma-1.5' //pass the parameter to the library
                    dockerPush 'yuisofull/demo:jma-1.5'
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
