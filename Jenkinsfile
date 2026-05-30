pipeline {

    agent any

    environment {
        IMAGE_NAME = "omarm710/flask-app"
    }

    stages {

        stage('Build Image') {
            steps {
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    bat """
                    echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                    """
                }
            }
        }

        stage('Push Image') {
            steps {
                bat "docker push %IMAGE_NAME%"
            }
        }

        stage('Run Container') {
            steps {
                bat """
                docker rm -f flask-container || exit 0
                docker run -d --name flask-container -p 5000:5000 %IMAGE_NAME%
                """
            }
        }
    }
}