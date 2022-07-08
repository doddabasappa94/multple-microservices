pipeline {
    agent any
    stages{
        stage('Build'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/doddabasappa94/devops-microservices']]])
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t doddabasappah/devops-microservices .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u doddabasappah -p ${dockerhubpwd}'

}
                   sh 'docker push doddabasappah/devops-microservices'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
