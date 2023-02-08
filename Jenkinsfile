pipeline {
    agent any
    environment {  
        DOCKER_HUB_LOGIN = credentials('docker-hub')
    }
    stages {
        stage('install dependencies') {
            agent{
                docker {
                    image 'node:erbium-alpine'
                    args '-u root:root'
                }
            }
            steps {
                echo "Init"
                sh 'npm install'
            }
        }
        stage('Unit Test') {
            agent{
                docker {
                    image 'node:erbium-alpine'
                    args '-u root:root'
                }
            }
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                sh 'npm run test'
                }
            }
        } 
        stage('Docker Build') {
            steps {
               sh 'docker build -t ms-frontend:1.0 frontend'
               sh 'docker build -t ms-products:1.0 products'
               sh 'docker build -t ms-shopping-cart:1.0 shopping-cart'
            }
        }

    } //--end stages

    // CLEAN WORKSPACES
    post { 
        always { 
            cleanWs()
        }
    }
}