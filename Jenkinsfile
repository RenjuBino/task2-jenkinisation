pipeline {
    agent any
   /* environment{
        YOUR_NAME = credentials("YOUR_NAME")
    }*/
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t renjubino/task2-dbjen db
                docker build -t renjubino/task2-appjenk flask-app
                '''
            }

        }
        stage('Push') {
            steps {
                sh '''
                docker push renjubino/task2-dbjen
                docker push renjubino/task2-appjenk
                '''
            }

        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f .
                sleep 60
                kubectl get services
                '''
            }

        }

    }

}