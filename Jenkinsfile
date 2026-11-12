
pipeline {
    agent any

    environment {
        PYTHON = 'C:/Users/unars/AppData/Local/Programs/Python/Python312/python.exe'
    }

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {

        stage('Source Code Checkout') {
            steps {
                echo 'Source code downloaded from GitHub'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '"%PYTHON%" -m pip install -r requirements.txt'
            }
        }

        stage('Compilation') {
            steps {
                bat '"%PYTHON%" -m compileall -q app.py test_app.py'
                echo 'Compilation successful'
            }
        }

        stage('Testing') {
            steps {
                bat '"%PYTHON%" -m pytest -v'
                echo 'Testing completed successfully'
            }
        }

        stage('Packaging') {
            steps {
                bat 'powershell -NoProfile -Command "if (!(Test-Path dist)) { New-Item -ItemType Directory dist }; Compress-Archive -Path app.py,requirements.txt -DestinationPath dist/python-app.zip -Force"'

                archiveArtifacts artifacts: 'dist/*.zip'

                echo 'Application packaged successfully'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline Successful!'
        }

        failure {
            echo 'CI/CD Pipeline Failed!'
        }
    }
}
