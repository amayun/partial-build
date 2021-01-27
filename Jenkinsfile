#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                nodejs('Node12_Latest') {
                    echo 'Hello World 2'
                    sh 'npm --version'
                }
            }
        }
    }
}
