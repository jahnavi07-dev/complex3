pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {

        stage('Source Code Checkout') {
            steps {
                checkout scm
                echo 'Source code downloaded from GitHub'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'py -m pip install -r requirements.txt'
            }
        }

        stage('Compilation') {
            steps {
                bat 'py -m compileall -q app.py test_app.py'
                echo 'Compilation successful'
            }
        }

        stage('Testing') {
            steps {
                bat 'py -m pytest -v'
                echo 'Testing completed'
            }
        }

        stage('Packaging') {
            steps {
                bat 'if not exist dist mkdir dist'
                bat 'py -c "import zipfile; z=zipfile.ZipFile(\'dist/python-app.zip\', \'w\'); z.write(\'app.py\'); z.write(\'requirements.txt\'); z.close()"'
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