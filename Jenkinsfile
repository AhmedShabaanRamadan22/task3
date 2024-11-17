pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh "docker build -t ahmed862/ahmedapp:${env.BUILD_NUMBER} ."
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh '''
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        docker push ahmed862/ahmedapp:${env.BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh "docker pull ahmed862/ahmedapp:${env.BUILD_NUMBER}"
                sh "docker run -d -p 60${env.PORT}:80 ahmed862/ahmedapp:${env.BUILD_NUMBER}"
            }
        }
    }

    post {
        always {
            slackSend(
                channel: 'ahmed',
                message: "Pipeline Status: ${currentBuild.currentResult} - Job: ${env.JOB_NAME} Build: ${env.BUILD_NUMBER} URL: ${env.BUILD_URL}"
            )
        }
    }
}
