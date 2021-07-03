pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Scan') {
            steps {
                // Install trivy
                sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin v0.18.3'
                sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl > html.tpl'
                
                // Scan all vuln levels
                sh 'mkdir -p reports'
                sh 'trivy filesystem --ignore-unfixed --vuln-type os,library --format template --template "@html.tpl" -o reports/nodjs-scan.html ./nodejs'
                publishHTML([
                    reportDir: 'reports',
                    reportFiles: "nodjs-scan.html",
                    reportName: 'Trivy Scan',
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true])

                // Scan again and fail on CRITICAL vulns
                sh 'trivy filesystem --ignore-unfixed --vuln-type os,library --exit-code 1 --severity CRITICAL ./nodejs'

            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}