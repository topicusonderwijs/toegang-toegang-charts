node() {
    def helmPath = 'frontend'

    stage('Checkout: source code') {
        git.checkout {
          stage = ''
        }

        println "Build number: '" + env.BUILD_NUMBER + "'"
    }

    catchError {
        stage("Validate") {
            helm.lint {
                stage = 'Validating helm charts'
                path = helmPath
            }
        }

        stage("Build & publish") {
            helm.pkg {
                stage = ''
                path = helmPath
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