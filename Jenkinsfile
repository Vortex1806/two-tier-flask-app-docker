@Library("SharedLib") _
pipeline {
    agent { label "PROD" };
    stages {
        stage("Clone") {
            steps {
                script{
                    clone("https://github.com/Vortex1806/two-tier-flask-app-docker", "master")
                }
            }
        }
        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test") {
            steps {
                echo "We are testing now....."
            }
        }
        stage("Push to docker hub") {
            steps{
                script {
                    docker_push("DockerHubCred", "two-tier-flask-app")
                }
            }
        }
        stage("Deploy") {
            steps {
                sh "docker compose build --no-cache flask-app"
                sh "docker compose up -d --force-recreate flask-app"
            }
        }
    }
    post {
        success {
            emailext(subject: "Build successful", body: "Your build was successful!!", to: 'shubhvora03@gmail.com')
        }
        failure {
            emailext(subject: "Build failed", body: "Your build was failed!!", to: 'shubhvora03@gmail.com')
        }
    }
}
