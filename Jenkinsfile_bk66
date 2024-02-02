pipeline {
    agent any
    
    environment {
        GCP_PROJECT = 'jenkins-413009'
        GCP_REPOSITORY = 'prod-laravel-api-base-image'
        IMAGE_TAG = "latest"
        GCP_SERVICE_ACCOUNT_CREDENTIALS = credentials('gcp-service-account-key')
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Build and Push Image') {
            steps {
                script {
                    // Authenticate with GCP using the service account key
                    withCredentials([googleServiceAccountCredentials(credentialsId: GCP_SERVICE_ACCOUNT_CREDENTIALS, variable: 'credentials('gcp-service-account-key')')]) {
                        sh 'gcloud auth activate-service-account --key-file=${GCP_SERVICE_ACCOUNT_CREDENTIALS}'
                        sh 'gcloud auth configure-docker'
                    }

                    // Build Docker image
                    sh "docker build -t gcr.io/${GCP_PROJECT}/${GCP_REPOSITORY}:${IMAGE_TAG} ."

                    // Push Docker image to GCP repository
                    sh "docker push gcr.io/${GCP_PROJECT}/${GCP_REPOSITORY}:${IMAGE_TAG}"
                }
            }
        }
    }
}

