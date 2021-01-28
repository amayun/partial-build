#!/usr/bin/env groovy

def APPS = []

pipeline {
    agent any

    /*triggers {
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
    }*/

    stages {
        /*stage('Init') {
            steps {
                script {
                    VALUESFILE = sh(returnStdout: true, script:'git diff $before $after --name-only')
                    LIST = VALUESFILE.split('\n')
                    def MAP = [:]
                    for(String file in LIST) {
                        MAP.put(file.split('/')[0], "build");
                    }
                    APPS = MAP.keySet()

                    echo "Changes in:${VALUESFILE}"
                    echo "application to build:${APPS}"
                }
            }
        }*/

        stage('PostInit') {
            steps {
                script {
                    def changeLogSets = currentBuild.changeSets
                    def changedFiles = []
                    for (entries in changeLogSets) {
                        for (entry in entries) {
                            for (file in entry.affectedFiles) {
                                changedFiles += "${file.path}"
                            }
                        }
                    }
                    echo "changedFiles: ${changedFiles}"
                    def changedPackages = changedFiles
                        .findAll { it.startsWith('packages') }
                        .collect { it.split('/')[1] }
                        .unique()

                    echo "changedPackages: ${changedPackages}"
                }
            }
        }
        stage('initial') {
            steps {
                script {
                   sh "echo 'Stage Initial'"
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
