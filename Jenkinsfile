pipeline {
    agent any
    stages {
        stage('Setup') {
            steps {
                // Update the system and install Python packages
                sh '''
                sudo apt update
                sudo apt install python3-pip -y
                sudo apt install python3-pytest -y  # Install pytest via apt
                '''
            }
        }
        stage('Build') {
            steps {
                // Install dependencies listed in requirements.txt
                sh 'pip3 install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                // Run pytest to execute tests
                sh 'pytest'
            }
        }
        stage('Deploy') {
            when {
                branch 'main'  // Only deploy if we are on the main branch
            }
            steps {
                // Deploy the Flask app
                sh 'nohup python3 app.py &'
            }
        }
    }
    post {
        success {
            // Send email notification on success
            emailext (
                to: 'ajfaraziz@gmail.com',
                subject: "Build Success: ${env.JOB_NAME}",
                body: "Build ${env.BUILD_NUMBER} succeeded!"
            )
        }
        failure {
            // Send email notification on failure
            emailext (
                to: 'ajfar@gmail.com',
                subject: "Build Failed: ${env.JOB_NAME}",
                body: "Build ${env.BUILD_NUMBER} failed."
            )
        }
    }
}
