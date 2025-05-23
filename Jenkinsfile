pipeline {
    agent any

    environment {
        VS_PATH = 'C:\\Program Files\\Microsoft Visual Studio\\2022\\Community\\MSBuild\\Current\\Bin\\MSBuild.exe'
        SOLUTION_FILE = 'test_repos.sln'
        TEST_EXECUTABLE = 'C:\\Users\\Marina\\vs_mkr_test1\\x64\\Debug\\test_repos.exe'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/MoroziukMaryna/SP2.git'
            }
        }

        stage('Build') {
            steps {
                bat "\"${VS_PATH}\" \"${SOLUTION_FILE}\" /p:Configuration=Debug /p:Platform=x64 /m"
            }
        }

        stage('Test') {
            steps {
                bat "\"${TEST_EXECUTABLE}\""
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}

