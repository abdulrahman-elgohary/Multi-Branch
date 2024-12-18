pipeline {
    agent {
        label 'my-slave' // Default agent for the pipeline
    }
    environment {
        KUBE_CONFIG = credentials('kubeconfig-file')
    }
    stages {
        stage('Build and Test') {
            steps {
                echo 'Running build and test on the slave node...'
            }
        }
        stage('Deploy to Kubernetes') {
            // agent { label 'master' } // Specify master node for this stage
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
                           export KUBECONFIG=${kubeconfig-file} && kubectl apply -f .
                           kubectl apply -f deployment.yaml -n ${namespace}
                        """
                    }
                }
            }
        }
    }
}
