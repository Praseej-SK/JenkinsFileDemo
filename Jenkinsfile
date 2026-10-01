pipeline{
    agent any

    environment {
        PATH = "C:\\Users\\Admin\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin;${env.PATH}"
    }

    parameters{
        string(
            name:'APP_PORT',
            defaultValue:'3000',
            description:'Server Port'
        )
    }

    environment{
        IMAGE_NAME='jenkins_demo-app'
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
                
                bat '"C:\\Users\\Admin\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" --version'
                bat '"C:\\Users\\Admin\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" ps'
            }
        }
        stage('Dependencies'){
            steps{
                bat 'npm install'
            }
        }
        stage('Test APP'){
            steps{
                bat 'npm test'
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