pipeline {
    agent any

    environment {
        NUGET_PATH = 'C:\\Tools\\nuget\\nuget.exe'
        VS_PATH = "C:\\Program Files\\Microsoft Visual Studio\\2022\\Community\\MSBuild\\Current\\Bin\\MSBuild.exe"
        SOLUTION_FILE = "test_repos.sln"
        TEST_EXECUTABLE = "x64\\Debug\\test_repos.exe"
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/MoroziukMaryna/SP4.git', branch: 'main'
            }
        }

        stage('Restore NuGet Packages') {
            steps {
                script {
                    try {
                        bat """
                            if not exist "${NUGET_PATH}" (
                                powershell -Command "Invoke-WebRequest -Uri 'https://dist.nuget.org/win-x86-commandline/latest/nuget.exe' -OutFile '${NUGET_PATH}'"
                            )
                        """
                        bat """
                            "${NUGET_PATH}" restore "${SOLUTION_FILE}"
                        """
                    } catch (Exception e) {
                        echo "Error restoring NuGet packages: ${e.message}"
                        currentBuild.result = 'FAILURE'
                        error("Pipeline stopped due to NuGet restore failure.")
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    bat "\"${VS_PATH}\" \"${SOLUTION_FILE}\" /p:Configuration=Debug /p:Platform=x64 /m"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    bat "\"${TEST_EXECUTABLE}\""
                }
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

