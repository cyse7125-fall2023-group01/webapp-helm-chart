pipeline {

    agent any

    triggers {
        GenericTrigger(
             genericVariables: [
                 [key: 'LATEST_RELEASE_TAG', value: '$.release.tag_name']
             ],
             token: 'webapp-helm-chart-token'
        )
    }

    stages {
        stage('Checkout') {
            steps {
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
        stage('Print Tag Details') {
            steps {
                script {
                    env.LATEST_RELEASE_TAG = sh(returnStdout: true, script: 'git describe --tags --abbrev=0').trim()
                    echo "Latest Release Tag: ${env.LATEST_RELEASE_TAG}"
                }
            }
        }
    }
}
