pipeline {
    agent {
        label 'my-slave'
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
                        
                        echo "Deploying to the namespace: ${namespace}"

                        sh """
                           kubectl --kubeconfig=/home/ubuntu/config apply -f deployment.yaml -n ${namespace}
                        """
                    }
                }
            }
        }
    }
}
