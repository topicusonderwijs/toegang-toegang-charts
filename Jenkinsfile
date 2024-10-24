node() {

    stage('Checkout: source code') {
        git.checkout {
          stage = ''
        }

        println "Build number: '" + env.BUILD_NUMBER + "'"
    }

    catchError {
        def helmPathProfielhuisFrontend = 'profielhuis/frontend'
        def helmPathProfielhuisBackend = 'profielhuis/backend'
        def helmPathProfielhuisHeavyBackend = 'profielhuis/heavy-backend'
        def helmPathProfielhuisBackendJobs = 'profielhuis/backend-jobs'
        def helmPathToegangMijn = 'toegang/mijn'
        def helmPathToegangBeheerReact = 'toegang/beheer-react'
        def helmPathToegangCore = 'toegang/core'
        def helmPathToegangApiV2 = 'toegang/api-v2'
        def helmPathToegangLogging = 'toegang/logging'
        def helmPathToegangReferrer = 'toegang/referrer'

        stage("Package and lint"){
            // old way
            packageHelmChart('frontend')
            packageHelmChart('backend')
            packageHelmChart('heavy-backend')
            packageHelmChart('backend-jobs')
            // new Way
            packageHelmChart(helmPathToegangMijn)
            packageHelmChart(helmPathToegangBeheerReact)
            packageHelmChart(helmPathToegangCore)
            packageHelmChart(helmPathToegangApiV2)
            packageHelmChart(helmPathToegangLogging)
            packageHelmChart(helmPathToegangReferrer)
            // Add new chart here
        }

        stage("Publish"){
            publishHelmCharts(helmPathProfielhuisFrontend)
            publishHelmCharts(helmPathProfielhuisBackend)
            publishHelmCharts(helmPathProfielhuisHeavyBackend)
            publishHelmCharts(helmPathProfielhuisBackendJobs)
            publishHelmCharts(helmPathToegangMijn)
            publishHelmCharts(helmPathToegangBeheerReact)
            publishHelmCharts(helmPathToegangCore)
            publishHelmCharts(helmPathToegangApiV2)
            publishHelmCharts(helmPathToegangLogging)
            publishHelmCharts(helmPathToegangReferrer)
            // Add new chart here
        }
    }

    notify {
        slackChannel = "#toegang-org-github"
    }
}

// helmPath must direct to the folder containing Chart.yaml
void packageHelmChart(String helmPath){
    dir("${helmPath}"){
        helm.lint {
            stage = "Validating helm chart " + helmPath
        }

        helm.pkg {
            stage = ""
        }
    }
}

void publishHelmCharts(String helmPath){
    dir("${helmPath}"){
        helm.upload {
            stage = ""				
            // path is the dir in helm-local on artifactory
            path = "toegang.org/${helmPath}"
        }
    }
}