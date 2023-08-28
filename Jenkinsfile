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
        
    

        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: 'github-token', 
                url: 'https://github.com/harshmandhu/manifest',
                branch: 'master'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    git credentialsId: 'github-token', 
                    url: 'https://github.com/harshmandhu/manifest',
                    branch: 'master';
                
                    
                    sh '''
                    cat deploy.yaml
                    sed -i "s+harshmandhu/cicd-e2e.*+harshmandhu/cicd-e2e:${BUILD_NUMBER}+g" deploy.yaml
                    git add deploy.yaml
                    git commit -m 'Updated the deploy.yaml | Jenkins Pipeline'
                    git push -u origin master
                    '''                        
                    
                }    
            }         
            
        }
    }


}


