pipeline {
    agent any

    stages {

        stage('Unzip Projects') {
            steps {
                sh 'rm -rf flask-app node-app html'
                sh 'unzip flask-app.zip -d flask-app'
                sh 'unzip node-app.zip -d node-app'
                sh 'mkdir -p html'
                sh 'cp index.html html/'
            }
        }

        stage('Build Flask Image') {
            steps {
                
                    sh '''
                        docker build -t flask-app-image ./flask-app/flask-app
                    '''
                
            }
        }

        stage('Build Node Image') {
            steps {
               
                    sh '''
                        docker build -t node-app-image ./node-app/node-app
                    '''
                
            }
        }

        stage('Run All Containers') {
            steps {
                sh '''
                    docker rm -f ${docker ps -aq} || true

                    docker run -d --name flask-container -p 5000:5000 flask-app-image
                    docker run -d --name node-container -p 3000:3000 node-app-image
                    docker run -d --name html-server -p 8085:80 -v ./html:/usr/share/nginx/html nginx
                '''
            }
        }
    }
}
