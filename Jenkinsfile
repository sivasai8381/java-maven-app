pipeline {
    agent any
    
    tools {
        maven "Maven3"
    }
    stages {
        stage('Clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Git clone') {
            steps {
                git branch: 'prod', url: 'https://github.com/sivasai8381/java-maven-app.git'
            }
        }
        stage('maven war file build') {
            steps {
               sh 'mvn clean package'
            }
        }
        stage('Docker images/conatiner remove') {
            steps {
                script{
                        sh '''docker stop javamavenappprod_container
                        docker rm javamavenappprod_container
                        docker rmi javamavenappprod aseemakram19/javamavenappprod:latest'''
                }  
            }
        }
        stage('Docker images - Push to dockerhub') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker', toolname: 'docker'){
                
                        sh '''docker build -t javamavenappprodprod .
                        docker tag javamavenappprod aseemakram19/javamavenappprod:latest
                        docker push  aseemakram19/javamavenappprod:latest'''
                      } 
                }
            }
        }
        stage('docker container of app') {
            steps {
               sh 'docker run -d -p 9002:8080 --name javamavenappprod_container -t aseemakram19/javamavenappprod:latest'
            }
        }
        
    }
}
