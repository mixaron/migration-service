pipeline {
    agent any

    
    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'v1.0', description: 'Tag for the Docker image')
    }

    tools {
        maven 'Maven 3.9.9'
        dockerTool 'Docker'
    }
    
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh 'docker build -t mixaron/migration-service:${params.IMAGE_TAG} .'
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-hub-credentials', url: '') {
                        sh 'docker push your-dockerhub-repo/migration-service:${params.IMAGE_TAG}'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
