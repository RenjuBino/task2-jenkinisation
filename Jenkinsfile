pipeline {
    agent any
    environment{
        YOUR_NAME = credentials("YOUR_NAME")
    }
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t renjubino/task2-dbjen db
                docker build -t renjubino/task2-appjenk flask-app
                docker build -t renjubino/task2-nginx nginx
                '''
            }

        }
        stage('Push') {
            steps {
                sh '''
                docker push renjubino/task2-dbjen
                docker push renjubino/task2-appjenk
                docker push renjubino/task2-nginx
                '''
            }

        }
        stage('Deploy') {
            steps {
                sh '''
                ssh jenkins@renju-deploy <<EOF
                export YOUR_NAME = ${YOUR_NAME}
                docker network rm task1-net && echo "Removed network" || echo "task1-net network not available"
                docker network create task1-net
                docker stop nginx && echo "Stopped nginx" || echo "nginx not running"
                docker rm nginx && echo "Removed nginx" || echo "nginx not available"
                docker stop flask-app && echo "Stopped flask-app" || echo "flask-app not running"
                docker rm flask-app && echo "Removed flask-app" || echo "flask-app not available"
                docker stop db && echo "Stopped db" || echo "db not running"
                docker rm db && echo "Removed db" || echo "db not available"
                docker run -d --name flask-app --network task1-net -e YOUR_NAME=${YOUR_NAME} renjubino/task2-appjenk
                docker run -d --name db --network task1-net renjubino/task2-dbjenk
                docker run -d --name nginx --network task1-net -p 80:80 renjubino/task2-nginx
                '''
            }

        }

    }

}