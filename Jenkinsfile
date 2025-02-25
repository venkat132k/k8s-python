pipeline {
    agent any

    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/manojcloud/k8s-python.git'
            }
        }
        
        stage('build docker image from python code'){
             steps {
                sh 'sudo docker build -t newimage /var/lib/jenkins/workspace/k8s-project'
                sh 'sudo docker tag newimage manoj167/newimage:latest'
                sh 'sudo docker tag newimage manoj167/newimage:${BUILD_NUMBER}'
            }
        }
        
        stage ('Push the Docker image') {
            steps {
                sh 'sudo docker image push manoj167/newimage:latest'
                sh 'sudo docker image push manoj167/newimage:${BUILD_NUMBER}'
            }
        }
        
        stage('Deploy on Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f /var/lib/jenkins/workspace/k8s-project/pod.yaml'
                sh 'sudo kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}
