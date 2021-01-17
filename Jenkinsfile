pipeline {
    agent any
    
    environment {
        git_commit_id = "${GIT_COMMIT}"
        hub_token = 'dockerhub_creds'
        registry = "13352/nginx_app"
    }
    stages {
        stage('Create app image') {
            steps {
                script {
                    AppImage = docker.build("$registry:$git_commit_id")  
                }
            }
        }
        stage('Push to HUB') {
            steps {
                script {
                    docker.withRegistry("https://registry.hub.docker.com", hub_token) {
                    AppImage.push("$git_commit_id")
                    }
                }
            }
        }
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $registry:$git_commit_id"
            }
        }
        stage('Run container') {
            steps{
                sh "sudo docker-compose up"
            }
        }
    }
}