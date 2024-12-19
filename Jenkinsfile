pipeline {
    agent any

    environment {
        KUBE_CONFIG = credentials('kubeconfig-file')
        DOCKER_IMAGE = 'gohary101/jenkins_repo1010:2.1'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'dev', url: 'https://github.com/abdulrahman-elgohary/Multi-Branch.git'
            }
        }


        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${DOCKER_IMAGE} Jenkins/Lab-23/.
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
                sed -i "s|image: .*|image: ${DOCKER_IMAGE}|" Jenkins/Lab-23/deployment.yml
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                 withKubeConfig([credentialsId: 'kubeconfig-file']) {
                    sh 'kubectl apply -f Jenkins/Lab-23/deployment.yml'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution complete!'
        }
    }
}

