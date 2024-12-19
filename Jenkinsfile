pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'gohary101/jenkins_repo1011:2.1'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/IbrahimAdell/Lab'
            }
        }


        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${DOCKER_IMAGE} .
                '''
            }
        }
        

        stage('Push Docker Image to Docker Hub') {
            steps {
    
                withCredentials([usernamePassword(credentialsId: 'Docker-Hub-Repo', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh '''
                    echo "${DOCKER_PASSWORD}" | docker login -u "${DOCKER_USERNAME}" --password-stdin
                    docker push ${DOCKER_IMAGE}
                    '''
                }
            }
        }

        
        stage('Update Deployment File') {
            steps {
                sh '''
                sed -i "s|image: .*|image: ${DOCKER_IMAGE}|" deployment.yml
                '''
            }
        }

    post {
        always {
            echo 'Pipeline execution complete!'
        }
    }
}

