library 'jenkins-build-unstable'

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
                script {
                    docker.image('aquasec/trivy:0.19.2').inside("-v /Users/faheemmemon/.cache/:/.cache/ --entrypoint ''") {
                        sh "mkdir -p reports"
                        sh """ls -Rlatr \$libraryPath"""
                        sh "trivy config ./manifests"
                        exitCode = sh(script: "trivy image --exit-code 2 --severity HIGH,CRITICAL ./manifests", returnStatus: true)

                        if ( exitCode == 2 ) {
                            unstable(message: "High or Critical security vulnerabilities identified by Trivy. More Info: https://intranet.amobee.com/display/DOS/Trivy+Scans")
                        }
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
