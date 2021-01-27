#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                nodejs('Node12_Latest') {
                    echo 'Hello World 3'
                    sh 'npm --version'
                }
            }
        }
    }
}
