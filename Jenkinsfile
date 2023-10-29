pipeline {
    agent any
    
    environment {
        GITHUB_TOKEN = credentials('SRI_HARSHA_GITHUB_TOKEN')
        NODEJS_HOME = tool name: 'NodeJS', type: 'Tool'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Setup Node.js') {
            steps {
                tool name: 'NodeJS', type: 'Tool'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh "npm install -g semantic-release @semantic-release/commit-analyzer @semantic-release/release-notes-generator @semantic-release/changelog -D @semantic-release/github @semantic-release/git"
            }
        }
        
        stage('Release Tag') {
            steps {
                withEnv(["GITHUB_TOKEN=${GITHUB_TOKEN}"]) {
                    sh "npx semantic-release"
                }
            }
        }
        
        stage('Update Charts.yaml') {
            steps {
                script {
                    def chartPath = "${WORKSPACE}"  // Update with your chart path
                    def releaseVersion = sh(script: "npx semantic-release version", returnStdout: true).trim()
                    
                    sh "sed -i 's/^version: .*/version: ${releaseVersion}/' ${chartPath}/Charts.yaml"
                }
            }
        }
    }
}




// pipeline {

//     agent any

//     environment {
//         IMAGE_NAME = 'quay.io/csye7125_webapp/webapp-container-registry/webapp-helm-chart'
//     }

//     triggers {
//         GenericTrigger(
//              genericVariables: [
//                  [key: 'LATEST_RELEASE_TAG', value: '$.release.tag_name']
//              ],
//              token: 'webapp-helm-chart-token'
//         )
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm: [
//                     $class: 'GitSCM',
//                     branches: [[name: '*/master']],
//                     doGenerateSubmoduleConfigurations: false,
//                     extensions: [[$class: 'CloneOption', noTags: false]],
//                     submoduleCfg: [],
//                     userRemoteConfigs: [[credentialsId: 'GITHUB_TOKEN',
//                     url: 'https://github.com/cyse7125-fall2023-group01/webapp-helm-chart.git']]
//                 ]
//             }
//         }
//         stage('Print Tag Details') {
//             steps {
//                 script {
//                     env.LATEST_RELEASE_TAG = sh(returnStdout: true, script: 'git describe --tags --abbrev=0').trim()
//                     echo "Latest Release Tag: ${env.LATEST_RELEASE_TAG}"
//                 }
//             }
//         }
//     }
// }
