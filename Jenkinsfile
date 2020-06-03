#!/usr/bin/env groovy

pipeline {
    agent any

    stages{
        stage("Sequential: clean, compile, test and build"){
            stages{

                stage("clean") {
                    steps {

                        sh "git clean -xdf"

                    }
                }

                stage("Init") {
                    steps { script { commitId = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim() } }
                }

                stage("Bouw code") {
                    steps { sh "./gradlew compileJava" }
                }

                stage("Run tests") {
                    steps { sh './gradlew test --no-daemon' }
                }

                stage("Build"){
                   steps{ sh './gradlew build -x test --no-daemon' }
                }

                stage("Checkstyle"){
                    steps{ sh './gradlew checkstyleMain --no-daemon' //run a gradle task
                        }
                }
            }
        }

        stage('deploy'){
            steps { sh './gradlew deployHeroku --no-daemon' }
        }
    }
}


