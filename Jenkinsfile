pipeline{
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhubcredentials')
        //credentials()- helper method
        // environemnt_name = credential ID (set whenn storing credential in Jenkins)
    }
    stages{
        stage("Build") {
            steps {
                bat 'docker build -t sahilrajputwins/helloworld .'
            }
        }

        stage("Test") {
            steps {
                bat 'docker run --name mycontainer -p 80:80 -d sahilrajputwins/helloworld'
            }
        }
        stage("Login") {
            steps {
                bat 'docker login -u $DOCKERHUB_CREDENTIALS --password-stdin'
                bat '''echo login completed'''
                
            }
        }
        stage("Package") {
            steps {
                bat 'docker push sahilrajputwins/helloworld' 
            }
        }
        stage("Deployed") {
            steps{
                bat '''echo deployed, build id: ${BUILD_ID}'''
            }
        }
    }

    
    
}