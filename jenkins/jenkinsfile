pipeline{
    agent {label 'slave'}
    stages{
        stage('preparation'){
            steps{
                sh 'git checkout master'
            }
        }
        stage('build image'){
            steps{
                sh 'docker build -f dockerfile . -t ahmedmsadek/jenkins_gitmaster:1.0'
            }
        }
        stage('push image'){
            steps{
                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable: USERNAME,passwordVariable: PASSWORD)]){
                    sh 'docker login --username $USERNAME --password $PASSWORD'
                    sh 'docker push ahmedmsadek/jenkins_gitmaster:1.0'
                }
            }
        }
        stage('deploy'){
            steps{
                sh 'docker run -d -p 8000:8000 ahmedmsadek/jenkins_gitmaster:1.0'
            }
        }
    }
}