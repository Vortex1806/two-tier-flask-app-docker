pipeline {
    agent any;
    stages {
        stage("Clone") {
            steps {
                git url: 'https://github.com/Vortex1806/two-tier-flask-app-docker', branch: 'master'
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
                withCredentials([usernamePassword(
                    credentialsId:"DockerHubCred",
                    passwordVariable: "dockerHubPassword",
                    usernameVariable: "dockerHubUser"
                    )
                ]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
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
}
