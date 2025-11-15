pipeline {
    agent any
    
    environment {
        REGION = 'us-central1'
        GCP_PROJECT_ID = 'byteeit-testing-project'
        REPOSITORY_NAME = 'llmops'
        IMAGE_NAME = 'multi-ai-agent'
        TAG = 'latest'
    }

    stages {
        stage('Clone GitHub Repo') {
            steps {
                script {
                    echo 'Cloning GitHub repo to Jenkins...'
                    checkout scmGit(
                        branches: [[name: '*/main']], 
                        extensions: [], 
                        userRemoteConfigs: [[
                            credentialsId: 'git-token', 
                            url: 'https://github.com/ahmedbasemdev/Multi-AI-Agent-using-Jenkins-SonarQube-FastAPI-GCP.git'
                        ]]
                    )
                }
            }
        }

        
    }
    
    post {
        always {
            // Clean up Docker images to save space
            sh 'docker system prune -f || true'
        }
        success {
            echo "Successfully built and deployed Medical RAG Chatbot to Cloud Run"
            // Archive security scan report
            archiveArtifacts artifacts: 'trivy-report.json', allowEmptyArchive: true
        }
        failure {
            echo "Pipeline failed. Please check the logs."
        }
    }
}