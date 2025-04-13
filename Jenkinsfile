pipeline {
    agent any
    environment {
        GOOGLE_APPLICATION_CREDENTIALS = credentials('TERRAFORM-AUTHE')
    }
    stages {
        stage('Clone') {
            steps {
                // Clone your GitHub repository
                git 'https://github.com/your-user/k8s-docker-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image and push to Google Container Registry
                    sh 'docker build -t gcr.io/gcp-dev-space/app-image:latest .'
                    sh 'gcloud auth configure-docker'
                    sh 'docker push gcr.io/gcp-dev-space/app-image:latest'
                }
            }
        }
        stage('Deploy to GKE') {
            steps {
                script {
                    // Authenticate with GKE cluster
                    sh 'gcloud container clusters get-credentials gke-cluster --zone us-central1-a --project gcp-dev-space'

                    // Apply Kubernetes YAML to deploy the app
                    sh 'kubectl apply -f k8s/deployment.yaml'
                }
            }
        }
    }
}
