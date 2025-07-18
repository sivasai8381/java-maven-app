pipeline {
    agent any
    
    tools {
        maven "maven"
    }
    stages {
        stage('Clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Git clone') {
            steps {
                git branch: 'test', url: 'https://github.com/sivasai8381/java-maven-app.git'
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
                        sh '''docker stop javamavenapptest_container
                        docker rm javamavenapptest_container
                        docker rmi javamavenapptest aseemakram19/javamavenapptest:latest'''
                }  
            }
        }
        stage('Docker images - Push to dockerhub') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker', toolname: 'docker'){
                
                        sh '''docker build -t javamavenapptesttest .
                        docker tag javamavenapptest aseemakram19/javamavenapptest:latest
                        docker push  aseemakram19/javamavenapptest:latest'''
                      } 
                }
            }
        }
        stage('docker container of app') {
            steps {
               sh 'docker run -d -p 9001:8080 --name javamavenapptest_container -t aseemakram19/javamavenapptest:latest'
            }
        }
        
    }
}
