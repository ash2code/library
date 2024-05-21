pipeline {
    agent any 

    environment {
        IMAGE_NAME = "ash2code/library"
        DOCKER_BUILDKIT = "1" // Enable BuildKit
    }

    stages {
        stage("code-checkout") {
            steps {
                git changelog: false, poll: false, url: 'https://github.com/ash2code/library-management-system.git'
                // Debugging: list files to verify Dockerfile presence
                sh 'ls -al'
            }
        }
        stage("docker-build") {
            steps {
                script {
                    def dockerfilePath = 'Dockerfile'
                    if (fileExists(dockerfilePath)) {
                        echo "Dockerfile exists at ${dockerfilePath}"
                        // Print Dockerfile contents for verification
                        sh "cat ${dockerfilePath}"
                        // Build Docker image
                        sh "docker build -t $IMAGE_NAME:${env.BRANCH_NAME} -f ${dockerfilePath} ."
                    } else {
                        error "Dockerfile does not exist at ${dockerfilePath}"
                    }
                }
            }
        }
        stage("images-list") {
            steps {
                sh "docker images -a"
            }
        }
        stage("docker-run") {
            steps {
                sh "docker container run -dt -p 5000:5000 $IMAGE_NAME:${env.BRANCH_NAME}"
            }
        }
    }
}
