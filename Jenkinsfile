pipeline {
    agent any

    stages {
        stage('Check Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Halfdone83/CI-CD-Jenkins---Selenium-WEB-Test'
            }
        }

        stage('Install .Net') {
            steps {
                bat '''
                    choco install dotnet-sdk -y --version=6.0.100
                '''
            }
        }

        stage('Install NuGet Packages') {
            steps {
                bat 'dotnet restore SeleniumBasicExercise.sln'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build SeleniumBasicExercise.sln'
            }
        }

        stage('Run TestProject1') {
            steps {
                bat 'dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=TestResults1.trx"'
            }
        }

        stage('Run TestProject2') {
            steps {
                bat 'dotnet test TestProject2/TestProject2.csproj --logger "trx;LogFileName=TestResults2.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults*.trx', allowEmptyArchive: true
            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults*.trx'
            ])
        }
    }
}
