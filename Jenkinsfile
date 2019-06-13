#!groovy

pipeline {
    agent any

    stages {

        stage('Add WP Engine remote') {
            steps {
                script { 
                    if (env.BRANCH_NAME == 'master') {
                        sh "git remote add production git@git.wpengine.com:production/devopsgroup.git"
                    } else if (env.BRANCH_NAME == 'dev') {
                       sh "git remote add staging git@git.wpengine.com:production/stagingdog.git"
                    } else {
                        sh "git remote add development git@git.wpengine.com:production/developmentdog.git"
                    }
                    sh "git remote -v"
                }
            }
        }

        stage('Deploy to development') {
            when {
                not {
                    anyOf {
                        branch 'master';
                        branch 'dev'
                    }
                }
            }
            steps {
                script { 
                    echo 'Deploy to development.'
                }
            }
        }

        stage('Deploy to staging') {
            when {
                branch 'dev'
            }
            steps {
                sshagent (credentials: ['wp-engine-deployment-key']) {
                    script {
                        echo 'Deploy to staging.'
                    }
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
    }

    post {
        cleanup{
            deleteDir()
        }
    }
}