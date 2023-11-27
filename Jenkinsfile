pipeline {
    agent any

    tools {
        // Define Maven tool installation
        maven "Maven"
    }

    stages {
        stage('checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Rojha-git/CI-CD-using-Docker.git'
            }
        }
        stage('Execute Maven') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Docker Build and Tag') {
            steps {
                sh 'docker build -t samplewebapp:latest .'
                sh 'docker tag samplewebapp rajf5/samplewebapp:latest'
                //sh 'docker tag samplewebapp nikhilnidhi/samplewebapp:$BUILD_NUMBER'
            }
        }

        stage('Publish image to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: "dockerHub", url: "https://index.docker.io/v1/"]) {
                    sh 'docker push rajf5/samplewebapp:latest'
                    //sh 'docker push nikhilnidhi/samplewebapp:$BUILD_NUMBER'
                }
            }
        }

        stage('Run Docker container on Jenkins Agent') {
            steps {
                sh "docker run -d -p 8003:8080 rajf5/samplewebapp"
            }
        }

        stage('Run Docker container on remote hosts') {
            steps {
                sh '''  
                ssh jenkins@172.31.28.25
                docker pull nikhilnidhi/samplewebapp
                docker run -d -p 8003:8080 nikhilnidhi/samplewebapp '''

                               "or"

                sh "ssh jenkins@172.31.28.25 "docker run -d -p 8003:8080 nikhilnidhi/samplewebapp"


                


                
            }
        }
    }
}
