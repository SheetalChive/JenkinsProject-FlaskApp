pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SheetalChive/JenkinsProject-FlaskApp.git'
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
                echo "Docker build bhi ho gaya"
            }
        }

        stage("Test") {
            steps {
                echo "Test ho gaya"
            }
        }

        stage("Push to docker hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]) {
                    sh '''
                    echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin
                    docker image tag two-tier-flask-app $dockerHubUser/two-tier-flask-app:latest
                    docker push $dockerHubUser/two-tier-flask-app:latest
                    '''
                }
            }
        }

        stage("Deploy") {
            steps {
                sh '''
                docker compose up -d --build flask-app               
                '''
            }
        }
    }
}
