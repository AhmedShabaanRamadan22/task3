pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t ahmed862/ahmedapp .'
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
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

 post {
        always {
            //Add channel name
            slackSend channel: 'ahmed',
            message: "Find Status of Pipeline:- ${currentBuild.currentResult} ${env.JOB_NAME} ${env.BUILD_NUMBER} ${BUILD_URL}"
        }
    }
}