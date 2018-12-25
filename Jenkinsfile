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
    triggers {cron('H/5 * * * 1-5')}
    triggers {pollSCM('H/5 * 0 0 1-5')}
    triggers {upstream(upstreamProjects:'job1,job2',threshold:hudson.model.Result.SUCCESS)}

    options{
        timeout(time:10,unit:'SECONDS')
        skipDefaultCheckout()
        timestamps()
    }
    stages {
        stage("Stage 1") {
            steps {
                echo 'Hello world!' 
                echo "env.BUILD_ID===> ${env.BUILD_ID}   env.JOB_NAME==> ${env.JOB_NAME}  env.JENKINS_URL===> ${env.JENKINS_URL}"
                echo "${env.CC}"
            }
        }
        // stage("Stage 2"){
        //     environment{
        //         BITBUCKET_COMMON_CREDS = credentials('04503d51-6ebc-4189-b36a-8655799bb929')
        //     }
        //     steps{
        //         echo "${env.BITBUCKET_COMMON_CREDS}"
        //         echo "${env.BITBUCKET_COMMON_CREDS_USR}"
        //         echo "${env.BITBUCKET_COMMON_CREDS_PSW}"
        //     }
        // }
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
                //echo "Hello stage5.."
                //echo "Hello, ${PERSON}, nice to meet you."
                //checkout scm
                //bat git([url: 'https://github.com/linfeng627/WebApplication1.git', branch: 'dev'])
                //powershell('ls')
                //powershell 'Write-Output "Hello, World!"'
                //PowerShell(". '.\\disk-usage.ps1'") //E:\Jenkins
                PowerShell("E:\\Jenkins\\cmdlmt.ps1")
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
        stage('Stage7') {
            steps {
                echo "检查nginx进程是否存在"
                script{step([$class: 'Mailer',notifyEveryUnstableBuild: true,recipients: "linfeng627@126.com",sendToIndividuals: true])}
            }
        }
        stage("Stage8"){
            steps{
                emailext(
                    subject:'${ENV,var="Job_Name"}-第${BUILD_NUMBER}次構建日志',
                    body:'${FILE,path="email.html"}',
                    to:"linfeng627@126.com"
                )
            }
        }
    }
    post {
        always {
            //junit '**/target/*.xml'
            echo "always.. "
            // emailext (
            //     subject: "'${env.JOB_NAME} [${env.BUILD_NUMBER}]' 更新正常",
            //     body: """
            //     详情：
            //     SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'
            //     状态：${env.JOB_NAME} jenkins 更新运行正常 
            //     URL ：${env.BUILD_URL}
            //     项目名称 ：${env.JOB_NAME} 
            //     项目更新进度：${env.BUILD_NUMBER}
            //     """,
            //     to: "linfeng627@126.com"//,//"${USERMAIL}",  BarryHu@k2workflow.com.cn
            //     //recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            //     )
            emailext (
    subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
    body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}/console'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>""",
    to: "linfeng627@126.com",
    recipientProviders: [[$class: 'DevelopersRecipientProvider']]
)
        }
        success{
            //
            echo "success..."
            //mail bcc: '', body: 'asdfffffffffffff.....success.......', cc: '', from: 'cchq.bi01@cimc.com', replyTo: '', subject: 'Jenkins Email', to: 'linfeng627@126.com'
        }
        failure {
            echo "failure..."
            //mail bcc: '', body: 'asdfffffffffffff............', cc: '', from: 'cchq.bi01@cimc.com', replyTo: '', subject: 'Jenkins Email', to: 'linfeng627@126.com'
        }
    }
}


def PowerShell(psCmd) {
    psCmd=psCmd.replaceAll("%", "%%")
    bat "powershell.exe -NonInteractive -ExecutionPolicy Bypass -Command \"\$ErrorActionPreference='Stop';[Console]::OutputEncoding=[System.Text.Encoding]::UTF8;$psCmd;EXIT \$global:LastExitCode\""
}