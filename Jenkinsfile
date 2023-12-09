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
                    sh "sed -i 's/version:.*/version: ${latestVersion}/' ${chartPath}"
                    
                    sh 'cat Chart.yaml'
                    // sh 'helm upgrade ${latestVersion} Chart.yaml'
                    
                    sh 'gh version'
                }
            }
        }
        
        // stage('Package and Release') {
        //     steps {
        //         script {
                    
        //             def chartPath = "${WORKSPACE}"  // Update with your chart path
        //             def releaseVersion = "${env.LATEST_RELEASE_TAG}"
                    
        //             sh 'ls ${chartPath}'
                    
        //             // Create a tarball of the repository
        //             sh "tar -czvf ${chartPath}/webapp-helm-chart.tar.gz ./*"
                    
        //             sh "gh release upload -R https://github.com/cyse7125-fall2023-group01/webapp-helm-chart.git ${releaseVersion} ${chartPath}/webapp-helm-chart.tar.gz"
                    
        //         }
        //     }
        // }
        stage('helm release or upgrade') {
            steps {
                script {
                    sh 'gcloud auth activate-service-account --key-file ~/gcp_sa_key.json'
                    sh 'gcloud container clusters get-credentials primary --region us-east1 --project csye7125-401203'
                    sh 'kubectl get pods'
                    sh 'kubectl create namespace webapp'
                    sh 'kubectl create namespace istio-system'
                    sh 'kubectl create namespace istio-ingress'
                    sh 'helm repo add istio https://istio-release.storage.googleapis.com/charts'
                    sh 'helm install istio-base istio/base -n istio-system --set defaultRevision=default'
                    sh 'helm install istiod istio/istiod -n istio-system'
                    sh 'helm install istio-ingress istio/gateway -n istio-ingress'
                    sh 'kubectl label namespace webapp istio-injection=enabled'
                    sh 'rm -rf /var/lib/jenkins/webapp-helm-chart'
                    sh 'mkdir /var/lib/jenkins/webapp-helm-chart'
                    sh 'cp -R . /var/lib/jenkins/webapp-helm-chart'
                    sh 'ls /var/lib/jenkins/webapp-helm-chart'
                    sh '/var/lib/jenkins/webapp-helm-chart.sh'
                    sh 'rm -rf /var/lib/jenkins/webapp-helm-chart'
                }
            }
        }
    }
}
