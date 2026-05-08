pipeline {
    agent any
    environment{
        SONAR_TOKEN=withCredentials('SONAR_TOKEN') // Replace with your Jenkins credential ID 
    }
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
        }

        stage('SonarCloud Analysis'){
            steps{
                bat '''
                    curl -L -o sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-windows.zip
                    tar -xf sonar-scanner.zip
                    set "PATH=%CD%\\sonar-scanner-5.0.1.3006-windows\\bin;%PATH%"
                    sonar-scanner.bat -Dsonar.login=%SONAR_TOKEN%
                '''
            }
        }
    }
}