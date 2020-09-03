pipeline{
    agent {label 'slave'}
    stages{
        stage('preparation'){
            steps{
                sh "git checkout ${env.GIT_BRANCH}"
            }
        }
        stage('build image'){
            steps{
                sh 'docker build -f dockerfile . -t ahmedmsadek/jenkinsslave:1.0m'
            }
        }
        stage('push image'){
            steps{
                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable: 'USERNAME',passwordVariable: 'PASSWORD')]){
                    sh 'docker login --username $USERNAME --password $PASSWORD'
                    sh 'docker push ahmedmsadek/jenkinsslave:1.0m'
                }
            }
        }
        stage('deploy'){
            steps{
                sh 'docker run -d -p 8000:8000 ahmedmsadek/jenkinsslave:1.0m'
            }
        }
    }
    post{
        success{
            slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME}  [${env.BUILD_NUMBER}]' (${env.BUILD_URL}console)")
        }
        failure{
            slackSend (color: '#E83009', message: "FAILURE: Job '${env.JOB_NAME} ${env.GIT_COMMIT} ${env.GIT_BRANCH} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}console)")
        }
        aborted{
            slackSend (color: '#E8E209', message: "ABORTED: Job '${env.JOB_NAME}  [${env.BUILD_NUMBER}]' (${env.BUILD_URL}console)")
        }
    }
}
