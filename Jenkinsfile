pipeline {
    agent {
        label 'My-Slave'
    } 
    environment {
        KUBE_CONFIG = credentials('kubeconfig-file')
    }
    stages {
        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig-file']) {
                    script {
                        // Dynamically set the namespace based on the branch name
                        def namespace = ''
                        if (env.BRANCH_NAME == 'main') {
                            namespace = 'production'
                        } else if (env.BRANCH_NAME == 'staging') {
                            namespace = 'staging'
                        } else {
                            namespace = 'dev'
                        }
                        
                        echo "Deploying to second namespace: ${namespace}"

                        sh """
                            kubectl apply -f deployment.yaml -n ${namespace}
                        """
                    }
                }
            }
        }
    }
}
