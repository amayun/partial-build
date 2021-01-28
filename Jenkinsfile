#!/usr/bin/env groovy

def APPS = []

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
        stage('Init') {
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
        }

        stage('PostInit') {
            steps {
                script {
                    echo "changes ${currentBuild.changeSets.toString()}"
                    //[[items: [entry]],]
                    /*def changeLogSets = currentBuild.changeSets
                    for (int i = 0; i < changeLogSets.size(); i++) {
                        def entries = changeLogSets[i].items
                        for (int j = 0; j < entries.length; j++) {
                            def entry = entries[j]
                            echo "${entry.commitId} by ${entry.author} on ${new Date(entry.timestamp)}: ${entry.msg}"
                            def files = new ArrayList(entry.affectedFiles)
                            for (int k = 0; k < files.size(); k++) {
                                def file = files[k]
                                echo "  ${file.editType.name} ${file.path}"
                            }
                        }
                    }*/
                }
            }
        }
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
