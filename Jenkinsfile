pipeline {
    agent any

    environment{
        EC2_USER = "ubuntu"
        EC2_HOST = "13.233.100.12"
        SSH_CREDENTIAL_ID = "ec2-ssh-key"
    }

    stages {
        stage("Build") {
            steps {
                echo "Start Building"
                sh "npm install"
                sh "npm run build"
                echo "Building Complete"
            }
        }    
        stage("Deploy"){
            steps {
                echo 'Starting Deployment on EC2...'
                // Use withCredentials to inject SSH private key
                sshagent(['ec2-ssh-key']) {
                sh '''

                echo "Copying build artifacts to EC2"
                scp -o StrictHostKeyChecking=no -r dist/ /var/www/html
                echo "restarting web server on ec2"
                sudo systemctl restart again
'''
                }
                echo "Deployment Completed"
            }
        }
    }
}
