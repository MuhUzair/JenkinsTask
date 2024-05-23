pipeline {
    agent any

    environment {
        GOOGLE_APPLICATION_CREDENTIALS = credentials('gcp-service-account-json')
    }

    stages {
        stage('Pull Code') {
            steps {
                git url: 'https://github.com/your-repo/your-project.git', branch: 'main'
            }
        }

        stage('Build Project') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package Application') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    def projectId = 'your-gcp-project-id'
                    def imageName = 'gcr.io/' + projectId + '/your-app'
                    
                    sh """
                    gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}
                    gcloud builds submit --tag ${imageName} .
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed'
        }
    }
}
