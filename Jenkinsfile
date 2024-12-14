pipeline {
    agent any
    stages {
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    def namespace = 'dev' // Update based on branch (dev, staging, production)
                    sh """
                        kubectl apply -f deployment.yaml -n ${namespace}
                    """
                }
            }
        }
    }
}
