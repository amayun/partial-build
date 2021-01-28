#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage('initial') {
            steps {
              echo 'Stage Initial'
            }
        }

        stage('build first') {
            when {
                changeset "/packages/first/**/*"
            }
            steps {
                nodejs('Node14_Latest') {
                    echo 'Stage Build First'
                    sh 'npm --version'
                }
            }
        }

        stage('build second') {
            when {
                changeset "/packages/second/**/*"
            }
            steps {
                nodejs('Node14_Latest') {
                    echo 'Stage Build Second'
                    sh 'npm --version'
                }
            }
        }
    }
}
