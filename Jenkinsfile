pipeline {
    agent { lebel "dev" };

    stages {
        stage("code") {
            steps {
                git url: "https://github.com/julkar09/flask-app-ecs.git", branch: "work-branch"
            }
        }

        stage("build") {
            steps {
                sh "docker build -t python-app:latest ."
            }
        }

        stage("test") {
            steps {
                echo "test is done"
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]) {
                    sh 'docker login -u $dockerHubUser -p $dockerHubPass'
                    sh 'docker image tag python-app:latest $dockerHubUser/python-app:00'
                    sh 'docker push $dockerHubUser/python-app:00'
                }
            }
        }

        stage("deploy") {
            steps {
                sh "docker compose up -d --build python_app"
            }
        }
    }
}
