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
            helm.lint {
                stage = 'Validating toegang/mijn helm charts'
                path = helmPathToegangMijn
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
            helm.pkg {
                stage = ''
                path = helmPathToegangMijn
            }
            helm.upload {
                stage = ''
                path = 'toegang.org'
            }
        }
    }

    notify {
        slackChannel = "#toegang-org-github"
    }
}