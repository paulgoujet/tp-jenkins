pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
            }
        }
        stage('Deploy') {
            steps {
                retry(3) {
                    sh '''
                        n=$(cat tentatives.txt 2>/dev/null || echo 0)
                        n=$((n+1))
                        echo $n > tentatives.txt
                        echo "Tentative de deploiement numero $n"
                        [ $n -ge 3 ]
                    '''
                }
                timeout(time: 3, unit: 'MINUTES') {
                    sh 'echo "Health check OK"'
                }
            }
        }
    }
    post {
        always {
            echo 'This will always run'
            sh 'rm -f tentatives.txt'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
        }
    }
}
