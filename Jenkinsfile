pipeline {
    agent any

    stages {
        stage('Checkout code') {
            // Checkout the repository
            steps {
                git(branch: 'main', url: 'https://github.com/viktoriyadi/JenkinsWebDriver_7_30')
            }
        }

        stage('Set up .NET CORE') {
            // Install dotnet
            steps {
                bat '''
                echo Downloading .Net 6 SDK
                curl -O https://download.visualstudio.microsoft.com/download/pr/23c7bf0d-e22d-4372-bcb2-292eb36a5238/11af494be409759f46b679ab22e65a58/dotnet-sdk-6.0.424-win-x64.exe
                echo Installing dotnet-sdk-6.0.424-win-x64.exe
                dotnet-sdk-6.0.424-win-x64.exe /quiet /norestart
                '''
            }
        }

        stage('Restore dependencies') {
            // Install dependencies
            steps {
                bat 'dotnet restore SeleniumBasicExercise.sln'
            }
        }

        stage('Build') {
            // Build the solution
            steps {
                bat 'dotnet build SeleniumBasicExercise.sln --configuration Release'
            }
        }

        stage('Run Tests TestProject1') {
            // Run tests for TestProject1
            steps {
                bat 'dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }

        stage('Run Tests TestProject2') {
            // Run tests for TestProject2
            steps {
                bat 'dotnet test TestProject2/TestProject2.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }

        stage('Run Tests TestProject3') {
            // Run tests for TestProject3
            steps {
                bat 'dotnet test TestProject3/TestProject3.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/*.trx'
            ])
        }
    }
}
