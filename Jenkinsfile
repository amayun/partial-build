#!/usr/bin/env groovy

def ALL = ['first', 'second', 'third', 'fourth']
def APPS = [];
def DEPS = [first: ['second', 'third'], second: ['third']];

pipeline {
    agent any

    /*triggers { // using parametrized build
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
            steps { // using parametrized build
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

        stage('Init') {
            steps {
                nodejs('Node14_Latest') {
                    script {
                        def baseRef = env.GIT_PREVIOUS_COMMIT
                        sh "npm --version"
                        sh "npm bin"
                        sh "npm install"
                        sh "\"\$(npm bin)\"/lerna list --all --parseable --since=${baseRef}"
                    }
                }
            }
        }

        stage('PostInit') {
            steps {
                script {
                    sh 'env'
                    /* GIT_PREVIOUS_COMMIT - в случае пуша в мастер это базовый коммит*/
                    def prevBuildResult = currentBuild.getPreviousBuild().result

                    echo "getBuildCauses(): ${currentBuild.getBuildCauses()[0].hashCode()}"
                    echo "getBuildVariables(): ${currentBuild.getBuildVariables()}"
                    echo "env: ${env}"

                    if(prevBuildResult == 'SUCCESS') {
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

                        APPS = changedPackages
                            .collect { [it, DEPS[it]] }
                            .flatten()
                            .unique()

                        echo "changedPackages: ${changedPackages}"
                    } else {
                        APPS = ALL
                    }

                    echo "APPS: ${APPS}"
                }
            }
        }
        stage('initial') {
            steps {
                nodejs('Node14_Latest') {
                    script {
                       sh "echo 'Stage Initial'"
                       sh 'npm --version'
                    }
                }
            }
        }

        stage('build based on changed packages') {
            steps {
                nodejs('Node14_Latest') {
                    script {
                        if(APPS.contains('first')) {
                            echo 'Build First'
                        }

                        if(APPS.contains('second')) {
                            echo 'Build Second'
                        }

                        if(APPS.contains('third')) {
                            echo 'Build Third'
                        }
                    }
                }
            }
        }

        stage('build based on single path') {
            when {
                changeset "packages/fourth/**/*"
            }
            steps {
                nodejs('Node14_Latest') {
                    echo 'Stage Build fourth'
                }
            }
        }
    }
}
