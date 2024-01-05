pipeline {
    agent any

    environment {
    DOCKERHUB_CREDENTIALS = credentials('jk-dh-tk')
}

    stages {
        stage('SCM Checkout') {
            steps {
                git branch: 'main', credentialsId: 'jk-gh-tk', url: 'https://github.com/Ingvarsvensen/jk-private-gh.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t zettertty/webapp:$BUILD_NUMBER .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Push Image') {
            steps {
                sh 'docker push zettertty/webapp:$BUILD_NUMBER'
            }
        }
        
        stage('Deploy Application') {
            steps {
                sh '''
                    #docker stop webapp_ctr
                    docker run --rm -d -p 3000:3000 --name webapp_ctr zettertty/webapp:${BUILD_NUMBER}
                '''
            }
        }
    }
}
