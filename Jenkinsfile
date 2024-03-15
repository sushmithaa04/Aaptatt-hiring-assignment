pipeline {
    agent any

    tools {
        maven "maven"
    }

    stages {
        stage('Build GitHub repository') {
            steps {
                git 'https://github.com/mkishornaik52/kishor-naik-Aaptatt-hiring-assignment.git'

                // Run Maven
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('Build Docker Images') {
            steps {
                script {
                    sh 'docker-compose build'
                    sh 'docker-compose down'
                }
            }
        }
        stage('Run Docker Containers') {
            steps {
                script {
                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                }
            }
        }
        stage('Clone VM') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh 'docker-compose push'
                    }
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
