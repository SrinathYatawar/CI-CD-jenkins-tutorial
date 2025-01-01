pipeline {
    agent any
    environment {
         NETLIFY_AUTH_TOKEN = credentials('pipeline-netlify')
        NETLIFY_SITE_ID = credentials('site_id')

    } 
     tools {
        nodejs "NodeJS" 
    }

     stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SrinathYatawar/CI-CD-jenkins-tutorial.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Deploy to Netlify') {
            steps {
              
                sh 'npx netlify deploy --prod --dir=build --site $NETLIFY_SITE_ID'
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