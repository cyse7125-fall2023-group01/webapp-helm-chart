pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm: [
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [[$class: 'CloneOption', noTags: false]],
                    submoduleCfg: [],
                    userRemoteConfigs: [[credentialsId: 'GITHUB_TOKEN',
                    url: 'https://github.com/cyse7125-fall2023-group01/webapp-helm-chart.git']]
                ]
                }
            }
        }

        stage('release') {
            steps {
                tool name: 'NodeJS', type: 'nodejs'
                tool name: 'NodeJS', type: 'nodejs'
                script {
                    
                    withCredentials([gitUsernamePassword(credentialsId: 'GITHUB_TOKEN', gitToolName: 'git')]) {
                        sh "npx semantic-release"
                    }
                }
            }
        }
        stage('post-release') {
            steps {
                script {
                    withCredentials([gitUsernamePassword(credentialsId: 'GITHUB_TOKEN', gitToolName: 'git')]) {
                        version_id= sh(returnStdout: true, script: "git describe --abbrev=0 --tags | tr -d 'v' ").trim()
                    }
                }
            }
        }
    }
}