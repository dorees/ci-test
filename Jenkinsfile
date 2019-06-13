#!groovy

pipeline {
    agent any

    stages {

        stage('Add WP Engine remote') {
            steps {
                script { 
                    if (env.BRANCH_NAME != 'master') {
                        sh "git remote add development git@git.wpengine.com:production/developmentdog.git"
                        sh "git remote add staging git@git.wpengine.com:production/stagingdog.git"
                    } else {
                        sh "git remote add production git@git.wpengine.com:production/devopsgroup.git"
                    }
                    sh "git remote -v"
                }
            }
        }

        stage('Deploy to development') {
            when {
                expression { BRANCH_NAME != /(master|dev)/ }
            }
            steps {
                script { 
                    echo 'Deploy to development.'
                    echo BRANCH_NAME
                }
            }
        }

        stage('Deploy to staging') {
            when {
                branch 'dev'
            }
            steps {
                script { 
                    echo 'Deploy to staging.'
                }
            }
        }

        stage('Deploy to production') {
            when {
                branch 'master'
            }
            steps {
                script { 
                    echo 'Deploy to production.'
                }
            }
        }














        // stage('Deployment') {
        //     steps {
        //         script { 
        //             if (env.BRANCH_NAME != 'master' || env.BRANCH_NAME != 'dev') {
        //                 echo 'Deploy to development'
        //                 echo env.BRANCH_NAME

        //             } else if (env.BRANCH_NAME == 'dev') {
        //                 echo 'Deploy to staging'

        //             } else if (env.BRANCH_NAME == 'master') {
        //                 echo 'Deploy to production'

        //             }
        //         }
        //     }
        // }
    }

    post {
        cleanup{
            deleteDir()
        }
    }
}