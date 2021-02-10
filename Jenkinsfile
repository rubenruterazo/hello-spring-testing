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


        stage('DependencyCheck') {
            steps {
                withGradle{
                    sh './gradlew dependencyCheckAnalyze'
                    /*configFileProvider(
                            [configFile(fileId: 'sonarqube-gradle-properties', targetLocation: 'gradle.properties')]) {
                                sh './gradlew sonarqube'
                    }*/

                }
            }
            post {
                always {
                    dependencyCheckPublisher pattern: 'build/reports/*.xml'
                }
            }
        }

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
                            sh './gradlew publish PTOKEN=$TOKEN'
                        }
                    }
                }
            }
        }
    }
}
