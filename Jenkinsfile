pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ramyavenkat23/rninfosoft-cicd-flask:latest"
    }

    stages {
        stage('Git Clone') {
            steps {
                echo 'Repository already cloned by Jenkins, skipping explicit clone...'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'pip3 install -r app/requirements.txt'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'pytest app/test_app.py'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }
    }
}
