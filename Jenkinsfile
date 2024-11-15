node() {

    stage('Checkout: source code') {
        git.checkout {
          stage = ''
        }

        println "Build number: '" + env.BUILD_NUMBER + "'"
    }

    catchError {
        def helmPathToegangMijn = 'toegang/mijn'
        def helmPathToegangBeheerReact = 'toegang/beheer-react'
        def helmPathToegangCore = 'toegang/core'
        def helmPathToegangApiV2 = 'toegang/api-v2'
        def helmPathToegangLogging = 'toegang/logging'
        def helmPathToegangReferrer = 'toegang/referrer'
        def helmPathToegangEck = 'toegang/eck'
        def helmPathToegangIdp = 'toegang/idp'
        def helmPathToegangBeheer = 'toegang/beheer'
        def helmPathProfielhuisRestApi = 'profielhuis/rest-api'
        def helmPathProfielhuisHeavyRestApi = 'profielhuis/heavy-rest-api'
        def helmPathProfielhuisJobs = 'profielhuis/jobs'

        stage("Package and lint"){
            packageHelmChart(helmPathToegangMijn)
            packageHelmChart(helmPathToegangBeheerReact)
            packageHelmChart(helmPathToegangCore)
            packageHelmChart(helmPathToegangApiV2)
            packageHelmChart(helmPathToegangLogging)
            packageHelmChart(helmPathToegangReferrer)
            packageHelmChart(helmPathToegangEck)
            packageHelmChart(helmPathToegangIdp)
            packageHelmChart(helmPathToegangBeheer)
            packageHelmChart(helmPathProfielhuisRestApi)
            packageHelmChart(helmPathProfielhuisHeavyRestApi)
            packageHelmChart(helmPathProfielhuisJobs)
            // Add new chart here
        }

        stage("Publish"){
            publishHelmCharts(helmPathToegangMijn)
            publishHelmCharts(helmPathToegangBeheerReact)
            publishHelmCharts(helmPathToegangCore)
            publishHelmCharts(helmPathToegangApiV2)
            publishHelmCharts(helmPathToegangLogging)
            publishHelmCharts(helmPathToegangReferrer)
            publishHelmCharts(helmPathToegangEck)
            publishHelmCharts(helmPathToegangIdp)
            publishHelmCharts(helmPathToegangBeheer)
            publishHelmCharts(helmPathProfielhuisRestApi)
            publishHelmCharts(helmPathProfielhuisHeavyRestApi)
            publishHelmCharts(helmPathProfielhuisJobs)
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