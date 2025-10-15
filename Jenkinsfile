pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "daksha16/dockerapp:latest"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Daksha-16/dockerapp.git'
            }
        }

        stage('Build Java Application') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t daksha16/dockerapp:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'tomcat-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                        docker push daksha16/dockerapp:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Docker image built and pushed successfully to Docker Hub!"
        }
        failure {
            echo "❌ Build or push failed!"
        }
    }
}
