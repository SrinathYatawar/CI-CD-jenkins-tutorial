pipeline {
    agent any
    
    environment {
        NETLIFY_AUTH_TOKEN = credentials('pipeline-netlify')
        NETLIFY_SITE_ID = credentials('site_id')
    }
    
    tools {
        nodejs "NodeJS"  // This must match exactly the name you gave in Jenkins configuration
    }

    
    stages {
        // First verify NodeJS installation
        stage('Verify Setup') {
            steps {
                bat 'node --version'
                bat 'npm --version'
            }
        }

       stage('Verify Credentials') {
            steps {
                echo "Testing Netlify credentials..."
                echo "Site ID is configured: ${NETLIFY_SITE_ID != null}"
                echo "Auth Token is configured: ${NETLIFY_AUTH_TOKEN != null}"
            }
        }
        
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SrinathYatawar/CI-CD-jenkins-tutorial.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        
        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }
        
        stage('Deploy to Netlify') {
            steps {
                bat 'npx netlify deploy --prod --dir=build --site %NETLIFY_SITE_ID%'
            }
        }
    }
    
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}