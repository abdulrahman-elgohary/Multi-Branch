pipeline {
    agent any
    environment {
        // Explicitly declare credentials
        MYKUBE = credentials('kubeconfig-file')
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
                withKubeConfig(credentialsId: 'kubeconfig-file') {
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
                           kubectl apply -f deployment.yaml -n ${namespace}
                        """
                    }
                }
            }
        }
    }
}
