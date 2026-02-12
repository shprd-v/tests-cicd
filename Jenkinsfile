pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/playwright/python:v1.47.0-jammy'
            args '--shm-size=2g'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                sh '''
                    pip install -r requirements.txt
                    playwright install --with-deps
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
