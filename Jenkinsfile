pipeline {
    agent any
    environment {
        // Use the 'TERRAFORM-AUTHE' credential ID for Google Cloud authentication
        GOOGLE_APPLICATION_CREDENTIALS = credentials('TERRAFORM-AUTHE')  // Ensure this matches your Jenkins credentials ID
    }
    stages {
        stage('Clone') {
            steps {
                // Clone the repository to get the app code
                git 'https://github.com/your-user/GKE-CICD-Pipeline.git'  // Your GitHub repo URL
            }
        }
        stage('Build Docker Image') {
            steps {
                // Build the Docker image using the Dockerfile in the repo
                sh 'docker build -t gcr.io/your-gcp-project-id/app-image:latest .'  // Replace with your GCP project ID
                sh 'gcloud auth configure-docker'  // Authenticate Docker to GCR
                sh 'docker push gcr.io/your-gcp-project-id/app-image:latest'  // Push image to GCR
            }
        }
        stage('Deploy to GKE') {
            steps {
                // Authenticate to GKE using 'gcloud' and deploy the app
                sh 'gcloud container clusters get-credentials gke-cluster --zone us-central1-a'  // Ensure your cluster zone matches
                sh 'kubectl apply -f k8s/deployment.yaml'  // Apply the deployment YAML for Kubernetes
            }
        }
    }
}
