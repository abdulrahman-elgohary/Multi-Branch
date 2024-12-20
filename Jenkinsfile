pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'gohary101/jenkins_repo1011:2.1'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the repository...'
                git branch: 'master', url: 'https://github.com/abdulrahman-elgohary/Argo-App.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building the Docker image...'
                sh '''
                docker build -t ${DOCKER_IMAGE} .
                '''
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo 'Pushing the Docker image to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'Docker-Hub-Repo', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh '''
                    echo "${DOCKER_PASSWORD}" | docker login -u "${DOCKER_USERNAME}" --password-stdin
                    docker push ${DOCKER_IMAGE}
                    docker rmi -f ${DOCKER_IMAGE}
                    docker logout
                    '''
                }
            }
        }

        stage('Update Deployment File') {
            steps {
                echo 'Updating the deployment file with the new Docker image...'
                sh '''
                sed -i "s|image: .*|image: hello|" deployment.yml
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution complete!'
        }
    }
}
