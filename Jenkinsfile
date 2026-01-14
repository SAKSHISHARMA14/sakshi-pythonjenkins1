pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo '=== CHECKOUT CODE ==='
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                echo '=== SETUP PYTHON ==='
                sh '''
                    python3.13 --version
                    python3.13 -m venv .venv
                    . .venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    pip install pytest flake8
                '''
            }
        }

        stage('Build - Run App') {
            steps {
                echo '=== BUILD STAGE ==='
                sh '''
                    . .venv/bin/activate
                    python main.py
                '''
            }
        }

        stage('Test - Pytest') {
            steps {
                echo '=== TEST STAGE ==='
                sh '''
                    . .venv/bin/activate
                    pytest tests/
                '''
            }
        }

        stage('Quality - Flake8') {
            steps {
                echo '=== FLAKE8 STAGE ==='
                sh '''
                    . .venv/bin/activate
                    flake8 .
                '''
            }
        }
    }

    post {
        success {
            echo '✅ PIPELINE PASSED: Build, Tests, and Flake8 successful'
        }
        failure {
            echo '❌ PIPELINE FAILED: Check logs above'
        }
    }
}
