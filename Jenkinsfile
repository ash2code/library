pipeline {
    agent any 

    environment {
        IMAGE_NAME = "ash2code/library"
    }

    stages {
        stage("code-checkout") {
            steps {
                git changelog: false, poll: false, url: 'https://github.com/ash2code/library-management-system.git'
            }
        }
        stage("docker-build") {
            steps {
                sh "docker build -t $IMAGE_NAME:${env.BRANCH_NAME} ."
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
