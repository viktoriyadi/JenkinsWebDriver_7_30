pipeline {
    agent any

    stages {
        stage('Checkout code') {
            steps {
                // Checkout the repository
                checkout([$class: 'GitSCM',
                    branches: [[name: 'main']],
                    userRemoteConfigs: [[url: 'https://github.com/viktoriyadi/JenkinsWebDriver_7_30']]
                ])
            }
        }

        stage('Set up .NET Core') {
            steps {
                script {
                    def downloadCmd = '''
                        echo Downloading .NET SDK
                        curl -k -O https://download.visualstudio.microsoft.com/download/pr/23c7bf0d-e22d-4372-bcb2-292eb36a5238/11af494be409759f46b679ab22e65a58/dotnet-sdk-6.0.424-win-x64.exe
                        if %ERRORLEVEL% neq 0 exit /b %ERRORLEVEL%
                    '''
                    def installCmd = '''
                        echo Installing .NET SDK
                        if exist dotnet-sdk-6.0.424-win-x64.exe (
                            dotnet-sdk-6.0.424-win-x64.exe /quiet /norestart
                        ) else (
                            echo ERROR: .NET SDK Installer not found!
                            exit /b 1
                        )
                    '''
                    bat script: downloadCmd
                    bat script: installCmd
                }
            }
        }

        stage('Restore dependencies') {
            steps {
                bat 'dotnet restore SeleniumWebDriver.sln'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build SeleniumWebDriver.sln --configuration Release'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'dotnet test SeleniumWebDriver.sln --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            junit '**/TestResults/*.trx'
        }
    }
}
