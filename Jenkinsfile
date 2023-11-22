pipeline {
    agent any
    environment{
        MYSQL_ROOT_PASSWORD = credentials("MYSQL_ROOT_PASSWORD")
    }
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
                sed -e 's,{{password}},'${MYSQL_ROOT_PASSWORD}',g;' db-password.yaml | kubectl apply -f -
                kubectl apply -f app-manifest.yaml
                kubectl apply -f nginx-config.yaml
                kubectl apply -f nginx-pod-config.yaml
                sleep 60
                kubectl get services
                '''
            }

        }

    }

}