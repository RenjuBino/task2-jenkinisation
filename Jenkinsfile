pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                cd ../..db
                docker build -t renjubino/task2-dbjen .
                '''
            }

        }
       /* stage('Push') {
            steps {
                sh '''
                docker push renjubino/task1jenk
                '''
            }

        }
        stage('Deploy') {
            steps {
                sh '''
                docker run -d -p 80:5500 --name task1 renjubino/task1jenk
                '''
            }

        }*/

    }

}