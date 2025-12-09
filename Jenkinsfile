pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = credentials('dockerhub-username')
        DOCKERHUB_PASSWORD = credentials('dockerhub-password')
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yourusername/devops-python-cicd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r app/requirements.txt'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'pytest app/tests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKERHUB_USERNAME}/python-cicd ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh """
                    echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin
                    docker push ${DOCKERHUB_USERNAME}/python-cicd
                """
            }
        }
    }

    post {
        always {
            echo "CI/CD Pipeline complete!"
        }
    }
}
