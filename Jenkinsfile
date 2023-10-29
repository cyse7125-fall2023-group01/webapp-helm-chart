pipeline {

    agent any

    environment {
        IMAGE_NAME = 'quay.io/csye7125_webapp/webapp-container-registry/webapp-helm-chart'
        GH_TOKEN = credentials('GH_TOKEN')
    }

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
        stage('make directory file') {
            steps {
                script {
                    sh 'ls'
                    sh 'rm -rf ./webapp-helm-chart-1'
                    sh 'rm -rf ./webapp-helm-chart'
                }
            }
        }
        stage('Edit Chart.yaml') {
            steps {
                script {
                    def chartPath = "${WORKSPACE}/Chart.yaml"  // Update with your chart path
                    def latestVersion = env.LATEST_RELEASE_TAG
                    
                    // Update Chart.yaml's apiVersion
                    sh "sed -i 's/^  apiVersion:.*/  apiVersion: ${latestVersion}/' ${chartPath}"
                    
                    sh 'gh version'
                }
            }
        }
        
        stage('Package and Release') {
            steps {
                script {
                    def chartPath = "${WORKSPACE}"  // Update with your chart path
                    def releaseVersion = "${env.LATEST_RELEASE_TAG}"
                    
                    sh 'ls ${chartPath}'
                    
                    // Create a tarball of the repository
                    sh "tar -czvf ${chartPath}/webapp-helm-chart.tar.gz ./*"
                    
                    sh "gh release upload -R https://github.com/cyse7125-fall2023-group01/webapp-helm-chart.git ${releaseVersion} ${chartPath}/webapp-helm-chart.tar.gz"
                    
                }
            }
        }

    }
}
