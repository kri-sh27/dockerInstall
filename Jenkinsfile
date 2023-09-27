pipeline {
    agent any

    stages {
        stage('Install Docker') {
            steps {
                script {
                    // Install Docker on the Jenkins agent (Linux-based)
                    sh 'curl -fsSL https://get.docker.com -o get-docker.sh'
                    sh 'sudo sh get-docker.sh'
                    sh 'sudo usermod -aG docker $USER'
                    sh 'sudo systemctl enable docker'
                    sh 'sudo systemctl start docker'
                }
            }
        }

        stage('Pull and Run Docker Image') {
            steps {
                script {
                    // Pull the hello-world Docker image
                    sh 'docker pull hello-world'

                    // Run the hello-world Docker image
                    sh 'docker run hello-world'
                }
            }
        }
    }

    post {
        always {
            // Clean up - stop and remove the Docker container
            script {
                sh 'docker stop $(docker ps -q)'
                sh 'docker rm $(docker ps -a -q)'
            }
        }
    }
}
