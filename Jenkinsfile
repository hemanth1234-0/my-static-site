pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hemanth753/my-static-site:latest'  // Replace with your Docker Hub repo
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-static-site')
                }
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        
                        // Tag the image for Docker Hub
                        sh "docker tag my-static-site ${DOCKER_IMAGE}"
                        
                        // Login to Docker Hub
                        sh "echo \$DOCKERHUB_PASSWORD | docker login -u \$DOCKERHUB_USERNAME --password-stdin"

                        // Push the image
                        sh "docker push ${DOCKER_IMAGE}"

                        // Logout (optional)
                        sh 'docker logout'
                    }
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

