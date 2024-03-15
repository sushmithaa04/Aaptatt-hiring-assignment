pipeline {
    agent any

    tools {
        // Use Maven tool named "maven"
        maven "maven"
    }

    stages {
        stage('Build GitHub repository') {
            steps {
                // Clone GitHub repository
                git branch: 'sushmithaa04-patch-1', url: 'https://github.com/sushmithaa04/Aaptatt-hiring-assignment.git'

                // Run Maven build
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('Build Docker Images') {
            steps {
                script {
                    // Build Docker images
                    sh 'sudo docker-compose build'

                    // Clean up existing Docker containers
                    sh 'sudo docker-compose down'
                }
            }
        }
        stage('Run Docker Containers') {
            steps {
                script {
                    // Stop existing Docker containers
                    sh 'sudo docker-compose down'

                    // Start Docker containers in detached mode
                    sh 'sudo docker-compose up -d'
                }
            }
        }
        stage('CloudVM') {
            steps {
                script {
                    // Push Docker images to Docker registry
                   withCredentials([usernameColonPassword(credentialsId: 'docker', variable: 'docker')]) {
    // some block

                        sh 'sudo docker-compose push'
                       
                   }
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
