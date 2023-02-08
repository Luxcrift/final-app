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
        stage('Docker Push to Docker-hub') {
            steps {
                sh 'docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW'
                sh 'docker push luxcrift/frontend:1.0'
                sh 'docker push luxcrift/products:1.0'
                sh 'docker push luxcrift/shopping-cart:1.0'
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