pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = '41a6a651-2ada-405d-866c-f9817d4f1cc7'
        DOCKER_IMAGE_NAME = 'news'
        MAIN_BRANCH = 'main'
        DOCKER_REGISTRY = 'monjj'
        imageTagApp = "build-${BUILD_NUMBER}-app"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    git branch: 'main', url:  'https://github.com/amanygamel/env.git'
                }
            }
        }

        stage('Build and Dockerize') {
            steps {
                // Build your application (adjust this based on your build tool, e.g., Maven, Gradle)

                // Build a Docker image
                sh "echo 'Amany*2023' | gpg --passphrase-fd 0 --batch --yes --no-tty --symmetric --cipher-algo AES256 --output /var/lib/jenkins/.docker/config.json.gpg /var/lib/jenkins/.docker/config.json"
                sh "echo 'Amany*2023' | docker login -u monjj --password-stdin"
                sh 'docker build -t ${DOCKER_REGISTRY}:${imageTagApp} .'
                sh "docker tag ${DOCKER_REGISTRY}:${imageTagApp} docker.io/${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${imageTagApp}"
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run Docker container
                    docker.image('my_alpine_image').run()
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "echo 'Amany*2023' | docker login -u monjj --password-stdin"
                sh "docker push docker.io/${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${imageTagApp}"
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

