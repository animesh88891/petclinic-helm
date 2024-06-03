pipeline {
    agent any
    environment {
        HELM_IMAGE_REPOSITORY = "cloud2023/petclinic"
        HELM_IMAGE_TAG = "2"
        HELM_RELEASE_NAME = "petclinic-app"
        HELM_CHART_PATH = "petclinic"
    }
    stages {
        stage('Build on k8') {
            steps {
                script {
                    // Print current directory
                    sh 'pwd'
                    
                    // Copy Helm charts
                    sh 'cp -R helm/* .'
                    
                    // List files to ensure correct copying
                    sh 'ls -ltr'
                    
                    // Print current directory again
                    sh 'pwd'
                    
                    // Run Helm upgrade/install command
                    sh """
                        /usr/local/bin/helm upgrade --install ${HELM_RELEASE_NAME} ${HELM_CHART_PATH} \
                        --set image.repository=${HELM_IMAGE_REPOSITORY} \
                        --set image.tag=${HELM_IMAGE_TAG}
                    """
                }
            }
            post {
                failure {
                    script {
                        // Print Helm release status in case of failure
                        sh "/usr/local/bin/helm status ${HELM_RELEASE_NAME} || true"
                        // Print last 100 lines of Kubernetes pod logs in case of failure
                        sh "kubectl logs -l app=petclinic --tail=100 || true"
                    }
                }
            }
        }
    }
}
