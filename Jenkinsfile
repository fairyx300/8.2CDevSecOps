pipeline {
    agent any
    stages{
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/fairyx300/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies'){
            steps{
                bat 'npm install'
            }
        }

        stage('Run Tests'){
            steps{
                bat 'npm test || exit /b 0' // Allows pipeline to continue despite test failures
            }
            post{
                always{
                    emailext(
                        mail to: 'felicitymorris30@gmail.com',
                        subject: "NPM Test Report",
                        body: "The NPM tests have completed with status: ${currentBuild.currentResult}. Please review the attached log for details on any failures.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report'){
            steps{
                // Ensure coverage report exists
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)'){
            steps{
                bat 'npm audit || exit /b 0' //This will show known CVEs in the output
            } 
            post{
                always{
                    emailext(
                        mail to: 'felicitymorris30@gmail.com',
                        subject: "NPM Audit Report",
                        body: "The NPM audit has completed with status: ${currentBuild.currentResult}. Please review the attached log for details on any vulnerabilities found.",
                        attachLog: true
                    )
                }
            }
        }
    }
}