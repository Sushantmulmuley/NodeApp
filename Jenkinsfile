pipeline {
    agent any

    environment{
        DEPLOY_DIR = '/var/www/html'
        SERVICE_NAME = 'nginx'
    }

    stages {
        stage("Build") {
            steps {
                echo 'Installing Dependencies...'
                sh 'npm install'
                
                echo 'Building Complete'
                sh 'npm run build'
                echo 'Build complete'
            }
        }    
        stage("Deploy"){
            steps {
                echo 'Deploying build to local web server...'
                // Use withCredentials to inject SSH private key
                
                sh '''

                echo "Copying build to $DEPLOY_DIR"
                sudo rm -rf $(DEPLOY_DIR)/*
                sudo cp -r dist/* $(DEPLOY_DIR)/

                echo "Restarting $SERVICE_NAME"
                sudo systemctl restart $(SERVICE_NAME)
'''
            echo 'Deployment completed.'
                
            }
        }
    }
}
