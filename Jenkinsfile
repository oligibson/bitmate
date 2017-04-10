#!groovy

node {

    def nodeHome = tool name: 'bitmate-generator-node', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
    env.PATH = "${nodeHome}/bin:${env.PATH}"
    currentBuild.result = "SUCCESS"

    try {

       stage('Check Environment'){
           sh "node -v"
           sh "npm -v"
       }

       stage('Checkout'){
            properties([pipelineTriggers([[$class: 'GitHubPushTrigger'], pollSCM('H/15 * * * *')])])
            checkout scm
       }

       stage('Setup Environment'){
            sh "echo '//registry.npmjs.org/:_authToken=${env.NPM_TOKEN}' > ~/.npmrc"
            sh "npm set progress=false"
            sh "npm install"
            sh "npm run unlinkAll"
            //sh "node test/cache/backup-node_modules.js"
            sh "rm -rf bitmate-generator/ generator-bitmate-*/"
            sh "git submodule update --init --recursive"
            //sh "node test/cache/restore-node_modules.js"
            sh "npm run linkAll"
       }

       stage('Testing'){
            sh 'npm run test'
       }
    }


    catch (err) {

        currentBuild.result = "FAILURE"

        throw err

    }

}

