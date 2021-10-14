pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Scan') {
            steps {
                docker.image('aquasec/trivy:0.19.2').inside("-v \$HOME/.cache/:/.cache/ --entrypoint ''") {
                    sh "mkdir -p reports"
                    sh "ls -lRas"
                    sh "trivy config --no-progress --timeout 30s ./manifests"
                    exitCode = sh(script: "trivy image --no-progress --timeout 30s --ignore-unfixed --exit-code 2 --severity HIGH,CRITICAL ./manifests", returnStatus: true)

                    if ( exitCode == 2 ) {
                        unstable(message: "High or Critical security vulnerabilities identified by Trivy. More Info: https://intranet.amobee.com/display/DOS/Trivy+Scans")
                    }
                }

            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
