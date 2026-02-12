pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                sh '''
                    python3 --version || echo "python3 not found"
                    pip3 --version || echo "pip3 not found"

                    pip3 install -r requirements.txt
                    playwright install
                '''
            }
        }

        stage('Run Playwright tests') {
            steps {
                sh '''
                    pytest tests/ \
                      --maxfail=1 \
                      --disable-warnings \
                      --html=playwright-report/index.html \
                      --self-contained-html
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true
        }
    }
}
