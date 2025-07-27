pipeline {
    agent any

    environment {
        EC2_HOST = "15.207.247.121"
        SSH_CREDENTIAL_ID = "ubuntukey"
        REMOTE_USER = "ubuntu"
        REMOTE_PATH = "/home/ubuntu/app"
        WEB_ROOT = "/var/www/html"
    }

    stages {
        stage("Build") {
            steps {
                echo "Installing dependencies"
                sh "npm install"
                echo "Building the app"
                sh "npm run build"
                echo "Build complete"
            }
        }

        stage("Deploy") {
            steps {
                echo "Starting Deployment"
                sshagent(credentials: [env.SSH_CREDENTIAL_ID]) {
                    sh """
                        echo "Creating remote directory"
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${EC2_HOST} "mkdir -p ${REMOTE_PATH}"
                        
                        echo "Copying Distribution files to remote path"
                        scp -o StrictHostKeyChecking=no -r dist/* ${REMOTE_USER}@${EC2_HOST}:${REMOTE_PATH}/
                        
                        echo "Restarting the nginx server"
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${EC2_HOST} '
                            sudo rm -rf ${WEB_ROOT}/*
                            sudo cp -r ${REMOTE_PATH}/* ${WEB_ROOT}/
                            sudo systemctl restart nginx
                        '
                    """
                }
            }
        }
    }
}
