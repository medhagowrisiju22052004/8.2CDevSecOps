pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/medhagowrisiju22052004/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
            post {
                always {
                    emailext(
                        to: 'medhagowrisiju@gmail.com',
                        subject: "Jenkins - Test Stage: ${currentBuild.currentResult}",
                        body: """
                            <h2>Test Stage Completed</h2>
                            <p><b>Job:</b> ${env.JOB_NAME}</p>
                            <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                            <p><b>Status:</b> ${currentBuild.currentResult}</p>
                            <p><b>Build URL:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                        """,
                        mimeType: 'text/html',
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
            post {
                always {
                    emailext(
                        to: 'medhagowrisiju@gmail.com',
                        subject: "Jenkins - Security Scan Stage: ${currentBuild.currentResult}",
                        body: """
                            <h2>Security Scan Stage Completed</h2>
                            <p><b>Job:</b> ${env.JOB_NAME}</p>
                            <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                            <p><b>Status:</b> ${currentBuild.currentResult}</p>
                            <p><b>Build URL:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                        """,
                        mimeType: 'text/html',
                        attachLog: true
                    )
                }
            }
        }
    }
}
