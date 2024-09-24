node() {
    def helmPathFrontend = 'frontend'
    def helmPathBackend = 'backend'
    def helmPathHeavyBackend = 'heavy-backend'
    def helmPathBackendJobs = 'backend-jobs'
    def helmPathToegangMijn = 'toegang/mijn'

    stage('Checkout: source code') {
        git.checkout {
          stage = ''
        }

        println "Build number: '" + env.BUILD_NUMBER + "'"
    }

    catchError {
        // deprecated old way (it is deprecated because it uploades all charts in the same folder)

        /*

        stage("Validate") {
            helm.lint {
                stage = 'Validating frontend helm charts'
                path = helmPathFrontend
            }
            helm.lint {
                stage = 'Validating backend helm charts'
                path = helmPathBackend
            }
            helm.lint {
                stage = 'Validating heavy-backend helm charts'
                path = helmPathHeavyBackend
            }
            helm.lint {
                stage = 'Validating backend-jobs helm charts'
                path = helmPathBackendJobs
            }
        }

        stage("Build & publish") {
            helm.pkg {
                stage = ''
                path = helmPathFrontend
            }
            helm.upload {
                stage = ''
                path = 'toegang.org/profielhuis'
            }
            helm.pkg {
                stage = ''
                path = helmPathBackend
            }
            helm.upload {
                stage = ''
                path = 'toegang.org/profielhuis'
            }
            helm.pkg {
                stage = ''
                path = helmPathHeavyBackend
            }
            helm.upload {
                stage = ''
                path = 'toegang.org/profielhuis'
            }
            helm.pkg {
                stage = ''
                path = helmPathBackendJobs
            }
            helm.upload {
                stage = ''
                path = 'toegang.org/profielhuis'
            }
        }

        */

        // new way (based on https://github.com/topicusonderwijs/par-schoolkassa-charts/blob/main/Jenkinsfile)

        stage("Package and lint"){
            packageHelmChart('toegang/mijn')
            //Add new chart here
        }

        stage("Publish"){
            publishHelmCharts('toegang/mijn')
            //Add new chart here
        }
    }

    notify {
        slackChannel = "#toegang-org-github"
    }
}

void packageHelmChart(String path){
    dir("${path}"){
        helm.lint {
            stage = ""
        }

        helm.pkg {
            stage = ""
        }
    }
}

void publishHelmCharts(String appPath){
    dir("${appPath}"){
        echo "appPath: ${appPath}"
        helm.upload {
            stage = ""				
            // path is the dir in helm-local on artifactory
            path = "toegang.org/${appPath}"
        }
    }
}