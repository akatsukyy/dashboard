pipeline {
    agent {
        label 'ansible-server'
    }
    stages {
        stage('Build images && Push to docker') {
            steps { 
                sh 'docker build -t $JOB_NAME:v1.$BUILD_ID .'
                sh 'docker tag $JOB_NAME:v1.$BUILD_ID kienkt/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker tag $JOB_NAME:v1.$BUILD_ID kienkt/$JOB_NAME:latest'
                sh 'docker push kienkt/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker push kienkt/$JOB_NAME:latest'
                sh 'docker rmi $JOB_NAME:v1.$BUILD_ID kienkt/$JOB_NAME:v1.$BUILD_ID kienkt/$JOB_NAME:latest'
            }
        }
        stage('Manage node server to create container') {
            steps {
                sh 'chmod +x /var/lib/jenkins/workspace/${JOB_NAME}@tmp/durable-*/script.sh.copy'
                sh 'ansible-playbook -i inventory.ini playbook.yml'
            }
        }
    }
}
