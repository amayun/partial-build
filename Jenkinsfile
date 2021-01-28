#!/usr/bin/env groovy

pipeline {
    agent any

    triggers {
        GenericTrigger(
            genericVariables: [
                [ key: 'ref', value: '$.ref' ],
                [ key: 'before', value: '$.before' ],
                [ key: 'after', value: '$.after' ]
            ],

            causeString: 'Triggered on $ref',

            token: 'maxsecure',
            tokenCredentialId: '',

            printContributedVariables: true,
            printPostContent: true,

            silentResponse: false,

            regexpFilterText: '$ref',
            regexpFilterExpression: 'refs/heads/master'
        )
    }

    stages {
        stage('initial') {
            steps {
                script {
                   sh "echo 'Stage Initial'"
                   sh "echo $ref"
                   sh "echo $before"
                   sh "echo $after"
                }
            }
        }

        stage('build first') {
            when {
                changeset "packages/first/**/*"
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
                changeset "packages/second/**/*"
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
