pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
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

        stage('Push the artifacts'){
           steps{
                withDockerRegistry(credentialsId: 'docker-cred', url: 'https://hub.docker.com/')
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
