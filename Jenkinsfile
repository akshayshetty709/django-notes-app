@Library("shared") _
pipeline {
    agent { label "shetty" }
    stages {
        stage("Hello") {
            steps {
                script {
                    hello()
                }
            }
        }
        stage("code") {
            steps {
                echo "this is cloning the code"
                git url: "https://github.com/akshayshetty709/django-notes-app.git", branch:"dev"
                echo "code cloned successfully"
            }
        }
        stage("Build") {
            steps {
                echo "this is building the code"
                sh "docker build -t notes-app:latest ."
            }
        }
        stage("Push") {
            steps {
                echo "this is pushing the image to dockerhub"
                withCredentials([usernamePassword(
                    'credentialsId':"Dockerhubcred",
                    passwordVariable:"DockerhubPass",
                    usernameVariable:"DockerhubUser")]){
                sh "docker login -u ${env.DockerhubUser} -p ${env.DockerhubPass}"
                sh "docker image tag notes-app:latest ${env.DockerhubUser}/notes-app:latest"
                sh "docker push ${env.DockerhubUser}/notes-app:latest "
                    }
            }
        }
        stage("Deploy") {
            steps {
                echo "this is Deploying the code"
                sh "docker compose up -d"
            }
        }
    }
}
