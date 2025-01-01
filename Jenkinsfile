pipeline {
    agent any
    
    environment {
        NETLIFY_AUTH_TOKEN = credentials('pipeline-netlify')
        NETLIFY_SITE_ID = credentials('site_id')
    }
    
    tools {
        nodejs "NodeJS"  // Exactly matching your Jenkins configuration
    }
    
    stages {
        stage('Verify Setup') {
            steps {
                // Explicitly verify node installation
                bat 'node --version'
                bat 'npm --version'
                // Install netlify-cli globally with specific error handling
                bat 'npm install -g netlify-cli --no-fund --no-audit'
            }
        }
        
        stage('Verify Credentials') {
            steps {
                script {
                    if (env.NETLIFY_AUTH_TOKEN == null || env.NETLIFY_SITE_ID == null) {
                        error "Credentials not found"
                    }
                    echo "Credentials loaded successfully"
                }
            }
        }
        
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SrinathYatawar/CI-CD-jenkins-tutorial.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                // Clean npm cache and verify node modules
                bat 'npm cache clean --force'
                bat 'rmdir /s /q node_modules' 
                bat 'npm install --verbose'
            }
        }
        
        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }
        
        stage('Deploy to Netlify') {
            steps {
                // Verify build directory exists
                bat 'if not exist "build" exit 1'
                
                // Use environment variables properly
                withEnv(["NETLIFY_AUTH_TOKEN=${env.NETLIFY_AUTH_TOKEN}", 
                        "NETLIFY_SITE_ID=${env.NETLIFY_SITE_ID}"]) {
                    bat '''
                        npx netlify-cli deploy --prod --dir=build --auth %NETLIFY_AUTH_TOKEN% --site %NETLIFY_SITE_ID%
                    '''
                }
            }
        }
    }
    
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
            // Add error logging
            bat 'npm config list'
            bat 'netlify-cli -v'
        }
    }
}