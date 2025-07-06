pipeline {
    agent any
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
            steps{
                echo "Starting Deployment on EC2"
                sh "scp -r -o strictCheckingOfKey=No ./dist/* /home/ubuntu/node-app/"
                sh "npm start"
                echo "Deployment Done"
            }
        }
    }
}
