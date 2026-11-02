pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Starting build'
                sh 'python3 --version'
                sh 'python3 -m compileall -q app.py'
                echo 'Build completed successfully'
            }
        }

        stage('Test') {
            steps {
                sh 'python3 -m venv .venv'
                sh '.venv/bin/pip install -r requirements.txt'
                sh 'mkdir -p reports'
                sh '.venv/bin/python -m pytest -q --junitxml=reports/junit.xml'
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true,
                  testResults: 'reports/junit.xml'
            echo 'Pipeline execution completed'
        }
        success {
            echo 'BUILD RESULT: SUCCESS'
        }
        failure {
            echo 'BUILD RESULT: FAILURE'
        }
    }
}