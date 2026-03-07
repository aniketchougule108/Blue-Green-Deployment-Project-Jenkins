pipeline {
    agent any

    stage('Pull Code') {
    steps {
        git branch: 'main', url: 'https://github.com/aniketchougule108/Blue-Green-Deployment-Project-Jenkins.git'
    }
}
        stage('Build') {
            steps {
                echo "Build completed"
            }
        }

        stage('Deploy') {
            steps {
                echo "Deployment stage"
            }
        }

    }
}