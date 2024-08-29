stages {
        stage('Build') {
            steps {
                sh 'docker build -t ahmed862/ahmedapp .'
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker push ahmed862/ahmedapp'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker pull ahmed862/ahmedapp'
                sh 'docker run -d -p 60:80 ahmed862/ahmedapp'
            }
        }
    }