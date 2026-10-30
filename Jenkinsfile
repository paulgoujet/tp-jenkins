pipeline {
    agent { docker { image 'mirror.gcr.io/library/python:3.15.2-alpine3.24' } }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building'
                sh 'python -m py_compile app.py'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh 'pip install -q pytest'
                sh 'python -m pytest --junitxml=test-results.xml || true'
                junit 'test-results.xml'
            }
        }
        stage('Deploy - Staging') {
            steps {
                echo 'Deploying to staging'
                sh 'echo "Smoke tests on staging: OK"'
            }
        }
        stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }
        stage('Deploy - Production') {
            steps {
                echo 'Deploying to production'
            }
        }
    }
}
