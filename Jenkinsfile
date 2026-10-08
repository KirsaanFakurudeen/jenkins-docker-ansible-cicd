pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/KirsaanFakurudeen/jenkins-docker-ansible-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t kirsaan/cicd-demo:${BUILD_NUMBER} .'
                sh 'docker tag kirsaan/cicd-demo:${BUILD_NUMBER} kirsaan/cicd-demo:latest'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push kirsaan/cicd-demo:${BUILD_NUMBER}'
                    sh 'docker push kirsaan/cicd-demo:latest'
                }
            }
        }

        stage('Deploy using Ansible') {
            steps {
                sh 'ansible-playbook -i ansible/inventory ansible/deploy.yml'
            }
        }
    }
}