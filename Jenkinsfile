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
                    sh './gradlew pitest'
                }
            }
            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                    pitmutation killRatioMustImprove: false, minimumKillRatio: 50.0, mutationStatsFile: '**/build/reports/pitest/*'
                    //'build/reports/pitest/*/'
                }
            }
        }
        stage('Check') {
            steps {
                withGradle{
                    sh './gradlew check'
                    withSonarQubeEnv(credentialsId: 'b44d3f91-20a5-46ab-9403-1515a4bb386a', installationName: 'local') {
                        // some block
                        sh './gradlew sonarqube'
                    }
                    /*configFileProvider(
                            [configFile(fileId: 'sonarqube-gradle-properties', targetLocation: 'gradle.properties')]) {
                                sh './gradlew sonarqube'
                    }*/

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
