pipeline {
    agent {label 'docker'}
    
    environment {
             DOCKERHUB_CREDENTIALS=credentials('dockerid')
             ACCOUNT_NAME="pratiak"
        IMAGE_REPO_NAME="java_jar"
        IMAGE_TAG="v1"
    }
    
   
    stages {
       
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Pratiak/docker-multistagebuild-java.git'     
            }
        } 
   
    // Building Docker images
    stage('Docker build') {
      steps{
            script {
                    sh "docker build -t ${ACCOUNT_NAME}/${IMAGE_REPO_NAME}:${IMAGE_TAG} ."
        }
      }
    }     
         stage('Logging to dockerhub') {
            steps {
                script {
                        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                        
                }
                 
            }
        }
        
        // Uploading Docker images into AWS ECR
    stage('Pushing to dockerhub') {
     steps{  
         script {
                sh """docker push ${ACCOUNT_NAME}/${IMAGE_REPO_NAME}:${IMAGE_TAG}"""
         }
        }
      }
      
      stage ('deploy to docker server'){
          steps{
                script{
                        sh "docker run ${ACCOUNT_NAME}/${IMAGE_REPO_NAME}:${IMAGE_TAG}" 
                }
          }
      }
      
    }
}
