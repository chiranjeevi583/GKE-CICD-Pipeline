pipeline {
    agent any
    
    environment {
        GOOGLE_APPLICATION_CREDENTIALS = credentials('TERRAFORM-AUTHE')
    }

    stages {
        // Stage 1: Clone the repository
        stage('Clone') {
            steps {
                // Ensure to use the correct branch 'main'
                git branch: 'main', url: 'https://github.com/chiranjeevi583/GKE-CICD-Pipeline.git'
            }
        }

        // Stage 2: Build Docker image
        stage('Build Docker Image') {
            steps {
                // Build Docker image
                sh 'docker build -t gcr.io/gcp-dev-space/app-image:latest .'

                // Authenticate Docker with Google Container Registry
                sh 'gcloud auth configure-docker'

                // Push Docker image to Google Container Registry
                sh 'docker push gcr.io/gcp-dev-space/app-image:latest'
            }
        }

        // Stage 3: Deploy to GKE
        stage('Deploy to GKE') {
            steps {
                // Authenticate with the GKE cluster
                sh 'gcloud container clusters get-credentials gke-cluster --zone us-central1-a'

                // Apply Kubernetes deployment YAML (update with your path if needed)
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }

    post {
        // Optional: Clean up after the build process
        always {
            cleanWs()
        }
    }
}
pipeline {
    agent any
    environment {
        GOOGLE_APPLICATION_CREDENTIALS = credentials('TERRAFORM-AUTHE')
    }
    stages {
        stage('Clone') {
            steps {
                // Clone your GitHub repository
               git 'https://github.com/chiranjeevi583/GKE-CICD-Pipeline.git'

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

                    // Create a Kubernetes Deployment directly using kubectl
                    sh '''
                    kubectl create deployment app-deployment --image=gcr.io/gcp-dev-space/app-image:latest
                    kubectl expose deployment app-deployment --type=LoadBalancer --port=80 --target-port=8080
                    '''
                }
            }
        }
    }
}
