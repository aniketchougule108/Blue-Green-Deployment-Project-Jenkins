pipeline {
    agent any

    stages {

        stage('Pull Code') {
            steps {
                git branch: 'main', url: 'https://github.com/aniketchougule108/Blue-Green-Deployment-Project-Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                echo "Build completed successfully"
            }
        }

        stage('Deploy') {
            steps {
                echo "Deployment stage running"
            }
        }

    }
}