pipeline{
    agent any

     tools{
        maven 'MAVEN'
    } 

    environment{
        PATH = "/usr/local/Cellar/maven/3.9.0/libexec:${PATH}"
        DOCKER_IMAGE = 'chinmaysharma/miniproj:latest'
        CONTAINER_NAME = 'calc_container'
    }

    stages{
        stage('Clone Git'){
            steps{
                git 'https://github.com/Chinmaysharmaaa/CALCULATOR_DevOps'
            }
        }

        stage('Build'){
            steps {
                sh "mvn clean package"
            }
        }
        
        stage('Test'){
            steps{
                 dir('/Users/chinmaysharma/Desktop/CALCULATOR_DevOps-master') {
                    sh "mvn test"
                }`
                
            }
        }
        stage('Containerize (Push to Dockerhub and pull from Dockerhub)') {
            steps {
                sh "docker build -t calculator ."
                withCredentials([usernamePassword(credentialsId: 'docker_hub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker tag calculator chinmaysharma/miniproj:latest'
                    sh 'docker push chinmaysharma/miniproj:latest'
                }
                sh 'docker pull chinmaysharma/miniproj:latest'

            }
        }
        stage('Ansible Deployment') {
            steps {
                script { 
                    sh 'ansible-playbook -i inventory deploy.yml'
                }
            }
        }

    }
}
