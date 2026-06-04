pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('jk-dh-pat')
    }
    stages {
        stage('Clonning Git Repository') {
            steps {
                git branch: 'main', credentialsId: 'jk-gh-pat', url: 'https://github.com/samuel-piza/jk-private-gh.git'
            }
        }
        
        stage('Building docker Image') {
            steps {
                sh "docker build -t samuelpiza/webapp:${BUILD_NUMBER} ."
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR --password-stdin"
            }
        }
        
        stage('Push Image') {
            steps {
                sh "docker push samuelpiza/webapp:${BUILD_NUMBER}"
            } // Faltava fechar o steps e a stage aqui
        }
        
        stage('Deploy Application') {
            steps {
                // Se o container antigo já existir rodando na máquina, pode ser necessário pará-lo antes
                sh "docker run --rm -d -p 3000:3000 --name webapp_ctr samuelpiza/webapp:${BUILD_NUMBER}"
            }
        }
    } // Fecha as stages
} // Fecha o pipeline
