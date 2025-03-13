pipeline{
    agent any
    environment {
        container_name= 'mycontainer'
    }
    stages{
        stage("Build") {
            steps {
                bat 'docker build -t sahilrajputwins/helloworld:%BUILD_ID% .'
            }
        }

        stage("Test") {
            steps {
                bat '''
                    docker stop %container_name% || echo building %container_name%
                    docker rm %container_name%
                '''
                bat 'docker run --name %container_name% -p 80:80 -d sahilrajputwins/helloworld:%BUILD_ID%'
            }
        }
        stage("Login") {
            steps {
                script{
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'DOCKER_USERNAME', usernamePassword: 'DOCKER_PASSWORD')]){
                        bat '''echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin'''
                    }
                }
                
            }
        }
        stage("Package") {
            steps {
                bat 'docker push sahilrajputwins/helloworld:%BUILD_ID%' 
            }
        }
        stage("Deployed") {
            steps{
                bat '''echo deployed, build id: %BUILD_ID%'''
            }
        }
    }
}