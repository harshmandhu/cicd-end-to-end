pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKERHUB_CREDENTIALS = credentials('docker-cred')
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git credentialsId: 'github-token', 
                url: 'https://github.com/harshmandhu/cicd-end-to-end',
                branch: 'main'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t harshmandhu/cicd-e2e:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    docker push harshmandhu/cicd-e2e:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
    }
}
