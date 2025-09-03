pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                // Builds the Docker image using the Dockerfile in your repo
                sh 'docker build -t adityakonda/python-cicd-app .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                // Securely uses the credentials you stored in Jenkins
                // 'dockerhub_credentials' is the ID you created in the Jenkins UI
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                    sh 'docker push adityakonda/python-cicd-app'
                }
            }
        }
        stage('Deploy Application') {
            steps {
                // Stops and removes any old version of the container
                sh 'docker stop python-app || true'
                sh 'docker rm python-app || true'
                // Runs the new version of your application container
                sh 'docker run -d --name python-app -p 80:80 adityakonda/python-cicd-app'
            }
        }
    }
}
