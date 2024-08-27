node() {
    def helmPathFrontend = 'frontend'
    def helmPathBackend = 'backend'
    def helmPathHeavyBackend = 'heavy-backend'
    def helmPathBackendJobs = 'backend-jobs'

    stage('Checkout: source code') {
        git.checkout {
          stage = ''
        }

        println "Build number: '" + env.BUILD_NUMBER + "'"
    }

    catchError {
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
    }

    notify {
        slackChannel = "#toegang-org-github"
    }
}