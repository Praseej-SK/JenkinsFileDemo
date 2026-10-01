pipeline{
    agent any
    

    parameters{
        string(
            name:'APP_PORT',
            defaultValue:'3000',
            description:'Server Port'
        )
    }

    environment {
    IMAGE_NAME = 'jenkins_demo-app'
    DOCKER_PATH = 'C:\\Users\\Admin\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin'
    NODE_PATH = 'C:\\Program Files\\nodejs'
    PATH = "${DOCKER_PATH};${NODE_PATH};${env.PATH}"
}

    stages{
        stage('Checkout'){
            steps{
                echo 'Checking out Source Code From Git Repo'
                checkout scm
            }
        }
        stage('Check Docker'){
            steps{
                
                 bat 'docker --version'
                bat 'docker ps'
            }
        }
        stage('Dependencies'){
            steps{
              
                bat 'node --version'
                bat 'npm.cmd --version'
             bat 'npm.cmd install'
            }
        }
        stage('Test APP'){
            steps{
                bat 'npm.cmd test'
            }
        }
        stage('Build'){
            steps{
                bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
            }
        }
        stage('Run Container'){
            steps{
                bat '''
                docker run -d --name node-app-%BUILD_NUMBER% -p %APP_PORT%:3000 %IMAGE_NAME%:%BUILD_NUMBER%
                '''
            }
        }
        stage('Verify'){
            steps{
                bat '''
                echo APP Deployed Successfully
                echo Open http://localhost:%APP_PORT%
                docker ps
                '''
            }
        }
    }
}