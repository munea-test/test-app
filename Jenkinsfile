pipeline {
    agent {
        any
    }
    environment {
        git_commit_id = "${GIT_COMMIT}"
    }
    stages {
        stage('Create app image') {
            steps {
                script {
                    AppImage = docker.build("13352/nginx_app$git_commit_id")  
                }
            }
        }
        stage('Push to HUB') {
            steps {
                script {
                    docker.withRegistry("https://registry.hub.docker.com", "docker_token") {
                    AppImage.push("$git_commit_id")
                    }
                }
            }
        }
        post {
            always {
                cleanWs()
            }
        }
    }
}