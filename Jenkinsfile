pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'myusername/my-static-site:latest'  // Replace with your Docker Hub repo
        DOCKERHUB_USERNAME = credentials('dockerhub-username') // Jenkins credentials ID
        DOCKERHUB_PASSWORD = credentials('dockerhub-password') // Jenkins credentials ID
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-static-site')
                }
            }
        }

        stage('Tag & Push to Docker Hub') {
            steps {
                script {
                    // Tag the image for Docker Hub
                    sh "docker tag my-static-site ${DOCKER_IMAGE}"

                    // Login to Docker Hub
                    sh "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"

                    // Push the image
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker rm -f static-site || true'
                    sh 'docker run -d -p 8034:80 --name static-site my-static-site'
                }
            }
        }
    }

    post {
        success {
            echo "Static site is running at http://43.205.199.168:8034"
        }
    }
}

