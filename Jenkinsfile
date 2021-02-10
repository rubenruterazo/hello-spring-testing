#!/usr/bin/env groovy
pipeline {
    agent any
    options {
        ansiColor('xterm')
    }
    //tools {
        //jdk 'OpenJDK-15.0.2'
    //}
    stages {




        stage('Build') {
            steps {
                withGradle{
                    //git url:'http://10.250.15.2:8929/root/hello-spring-testing', branch:'master'
                    sh './gradlew assemble'

                }
            }
            post {
                success {
                    archiveArtifacts 'build/libs/*.jar'
                    //7db4ace2-21cf-42f8-8d18-7fb8f3b04110
                    withCredentials([string(credentialsId: '7db4ace2-21cf-42f8-8d18-7fb8f3b04110', usernameVariable: 'USER')
                                     ,string(credentialsId: '7db4ace2-21cf-42f8-8d18-7fb8f3b04110',passwordVariable: 'PASS')]) {
                        withGradle{
                            sh './gradlew publish'
                        }
                    }
                }
            }
        }
    }
}
