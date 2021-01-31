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
        stage('Test') {
            steps {
                withGradle{
                    sh './gradlew clean test'
                }  
            }
            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                    jacoco( 
                        execPattern: 'build/*',
                        //classPattern: 'build/classes',
                        sourcePattern: 'src/main/java',
                        //exclusionPattern: 'src/test*'
                    )
                }
            }
        }
    }
}
