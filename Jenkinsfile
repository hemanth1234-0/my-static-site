pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Assuming Jenkins pipeline is set to build from SCM, or clone manually
                echo "Code cloned from Git repo"
            }
        }

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
                    
               
                    sh 'docker run -d -p 8034:80 --name static-site my-static-site'
                }
            }
        }
    }

    post {
        success {
            echo "Static site is running at http://<your-server-ip>:8080"
        }
    }
}


