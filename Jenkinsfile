pipeline {
    agent { docker { image 'mirror.gcr.io/library/python:3.15.2-alpine3.24' } }
    stages {
        stage('Build') {
            steps {
                sh 'mkdir -p build/libs && tar -czf build/libs/app.tar.gz app.py'
            }
        }
        stage('Test') {
            steps {
                sh 'pip install -q pytest'
                sh 'python -m pytest --junitxml=build/reports/tests.xml || true'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'build/libs/**/*.tar.gz', fingerprint: true
            junit 'build/reports/**/*.xml'
        }
    }
}
