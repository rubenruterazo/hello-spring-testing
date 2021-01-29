#!/usr/bin/env groovy
pipeline {
    agent any
    options {
        ansiColor('xterm')
    }
    tools {
        //jdk 'OpenJDK-15.0.2'
    }
    stages {
        stage('Build') {
            steps {
                //git url:'http://10.250.15.2:8929/root/hello-spring-testing', branch:'master'
                sh './gradlew assemble'
            }
        }
        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }
    }
}
