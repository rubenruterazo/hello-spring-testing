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
                }
            }
        }

        stage('Check') {
            steps {
                withGradle{
                    sh './gradlew check'
                }
            }
            post {
                always {
                    recordIssues(
                            enabledForFailure: true,
                            tools: [java(), spotBugs( pattern: 'build/reports/spotbugs/*.xml', reportEncoding: 'UTF-8')]
                    )
                }
            }
        }

    }
}
