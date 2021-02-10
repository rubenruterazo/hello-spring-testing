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
                    withCredentials([string(credentialsId: 'gitLabPrivateToken', variable: 'TOKEN')]) {
                        withGradle{
                            sh './gradlew publish'
                        }
                    }
                }
            }
        }
    }
}
