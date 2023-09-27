pipeline {
    agent any

    stages {
        stage('Install Docker') {
            steps {
                script {
                    // Configure sudo to not require a password for specific commands
                    sh 'echo "$(whoami) ALL=(ALL) NOPASSWD: /usr/bin/apt-get, /usr/bin/curl, /usr/bin/install, /usr/bin/gpg, /usr/bin/tee" | sudo tee /etc/sudoers.d/jenkins'
                    sh 'sudo chmod 440 /etc/sudoers.d/jenkins'

                    // Add Docker's official GPG key
                    sh 'sudo su'
                    sh 'sudo apt-get update'
                    sh 'sudo apt-get install -y ca-certificates curl gnupg'
                    sh 'sudo install -m 0755 -d /etc/apt/keyrings'
                    sh 'curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg'
                    sh 'sudo chmod a+r /etc/apt/keyrings/docker.gpg'

                    // Add the Docker repository to Apt sources
                    def versionCodename = sh(script: '(. /etc/os-release && echo "$VERSION_CODENAME")', returnStdout: true).trim()
                    sh 'echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu ${versionCodename} stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null'

                    // Update Apt repository
                    sh 'sudo apt-get update'
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
