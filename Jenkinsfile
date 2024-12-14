pipeline {
    agent any
    
    environment {
        KUBE_CONFIG = credentials('kubeconfig-file')
    }
    stages {
        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig-file']) {
                script {
                    def namespace = 'master' // Update based on branch (dev, staging, production)
                    sh """
                        kubectl apply -f deployment.yaml -n ${namespace}
                    """
                    }
                }
            }
        }
    }
}
