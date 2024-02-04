pipeline {
    agent any
    
    environment {
        GCP_PROJECT = 'jenkins-413009'
        GCP_REPOSITORY = 'prod-laravel-api-base-image'
        IMAGE_TAG = "latest"
	//GCP_SERVICE_ACCOUNT_CREDENTIALS = credentials('gcp-service-account-key')
        GCP_CREDS_ID = '2515354e-4e86-47f4-b57d-d4e72bd4b844'  // Define GCP_CREDS_ID
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
                    //withCredentials([googleServiceAccountCredentials(credentialsId: GCP_CREDS_ID, variable: 'GCP_SERVICE_ACCOUNT_CREDENTIALS')]) {
                    //withCredentials([file(credentialsId: 'gcp-service-account-key', variable: 'GCP_SERVICE_ACCOUNT_CREDENTIALS')]) {
                      withCredentials([file(credentialsId: '2515354e-4e86-47f4-b57d-d4e72bd4b844', variable: 'GCP_SERVICE_ACCOUNT_CREDENTIALS')]) {
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

