pipeline {
    agent any
    stages{
        stage("code checkout"){
            steps{
                git 'https://github.com/mohit9816/docker-project.git'
            }
        }
        stage("image build"){
            steps{
                sh 'docker build -t mohitv62/docker-test:v$BUILD_ID .'
                sh 'docker image tag mohitv62/docker-test:v$BUILD_ID mohitv62/docker-test:latest'
            }
        }
        stage("image push"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub-id', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh "docker login -u ${user} -p ${pass}"
                    sh 'docker image push mohitv62/docker-test:v$BUILD_ID'
                    sh 'docker image push mohitv62/docker-test'
                    sh 'docker image rmi mohitv62/docker-test:v$BUILD_ID mohitv62/docker-test:latest'
                }
            }
        }
        stage("container creating"){
            steps{
                sh 'docker run -itd --name docker-test -p 3000:3000 mohitv62/docker-test:latest'
            }
        }
    }
}
