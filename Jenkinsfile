#!/usr/bin/env groovy

pipeline {
    agent any
    environment{
        CC = 'clang'
    }
    parameters {
        string(name: 'Greeting', defaultValue: 'Hello', description: 'How should I greet the world?')
        booleanParam(name: 'DEBUG_BUILD', defaultValue: true, description: 'aaaaaaaaaa description')
    }
    triggers {
        cron('H/5 * * * 1-5')
    }
    options{
        timeout(time:10,unit:'SECONDS')
    }
    stages {
        stage('Stage 1') {
            steps {
                echo 'Hello world!' 
                echo "env.BUILD_ID===> ${env.BUILD_ID}   env.JOB_NAME==> ${env.JOB_NAME}  env.JENKINS_URL===> ${env.JENKINS_URL}"
                echo "${env.CC}"
            }
        }
        stage("Stage 2"){
            environment{
                BITBUCKET_COMMON_CREDS = credentials('db4045be-25ee-4a7f-ae34-0b8ea3faa307')
            }
            steps{
                echo "${env.BITBUCKET_COMMON_CREDS}"
                echo "${env.BITBUCKET_COMMON_CREDS_USR}"
                echo "${env.BITBUCKET_COMMON_CREDS_PSW}"
            }
        }
        stage("Stage3"){
            steps{
                echo "${params.Greeting} World!"
            }
        }
        stage("Stage4"){
            steps{
                script {
                    def browsers = ['chrome', 'firefox']
                    for (int i = 0; i < browsers.size(); ++i) {
                        echo "Testing the ${browsers[i]} browser"
                    }
                }
            }
        }
        stage("Stage5"){
            //input {
                //message "Should we continue?"
                //ok "Yes, we should."
                //submitter "alice,bob,barryhu"
                //parameters {
                    //string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                //}
            //}
            steps{
                echo "Hello, ${PERSON}, nice to meet you."
                //checkout scm
                //bat git([url: 'https://github.com/linfeng627/WebApplication1.git', branch: 'dev'])
            }
        }
        stage("Stage6"){
            when {
                branch 'production'
                anyOf {
                    environment name: 'DEPLOY_TO', value: 'production'
                    environment name: 'DEPLOY_TO', value: 'staging'
                }
            }
            steps{
                echo "production environment..."
            }
        }
    }
    post {
        always {
            //junit '**/target/*.xml'
            echo "always.."
        }
        success{
            //
            echo "success..."
        }
        failure {
            //mail to: linfeng627@126.com, subject: 'The Pipeline failed . pls fixed'
            echo "failure..."
        }
    }
}
