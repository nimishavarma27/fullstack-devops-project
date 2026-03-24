pipeline {
    agent any

    environment {
        DOCKER_USER = "nimishav27"
        FRONTEND_IMAGE = "${DOCKER_USER}/2023bcd0052_frontend"
        BACKEND_IMAGE = "${DOCKER_USER}/2023bcd0052_backend"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/nimishavarma27/fullstack-devops-project.git'
            }
        }

        //  Login FIRST (important fix)
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        //  Then Build
        stage('Build Images') {
            steps {
                sh '''
                docker build -t $FRONTEND_IMAGE ./frontend
                docker build -t $BACKEND_IMAGE ./backend
                '''
            }
        }

        //  Then Push
        stage('Push Images') {
            steps {
                sh '''
                docker push $FRONTEND_IMAGE
                docker push $BACKEND_IMAGE
                '''
            }
        }
    }
}
