pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Install any dependencies globally if they are not already installed
                sh 'sudo pip3 install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                // Run pytest for unit testing
                sh 'pytest'
            }
        }
        stage('Deploy') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                // Restart your Flask application service
                sh 'sudo systemctl restart flaskapp.service'
            }
        }
    }

    post {
        always {
            mail to: 'ajfaraziz@gmailcom',
                 subject: "Build ${currentBuild.fullDisplayName} - ${currentBuild.currentResult}",
                 body: "The build ${currentBuild.fullDisplayName} finished with status: ${currentBuild.currentResult}"
        }
    }
}
