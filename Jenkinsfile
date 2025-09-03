pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-static-site')
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove any existing container
                    sh 'docker rm -f static-site || true'

                    // Run new container
                    sh 'docker run -d -p 8034:80 --name static-site my-static-site'
                }
            }
        }
    }

    post {
        success {
            echo "Static site is running at http://<your-server-ip>:8034"
        }
    }
}

