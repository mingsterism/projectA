pipeline {
    agent any
    stages {
        stage('Initialize'){
            steps {
                script {
                    def dockerHome = tool 'docker'
                    env.PATH = "${dockerHome}/bin:${env.PATH}"   
                }
    
            }
        }
        
           stage('Get docker') {
            steps {  
                sh 'curl -LO "https://download.docker.com/linux/static/stable/x86_64/docker-18.06.3-ce.tgz"'
                sh 'tar xzvf ./docker-18.06.3-ce.tgz'
                sh 'ls'
                sh 'chmod u+x ./docker'
                sh 'cp docker/* /usr/bin/'
                sh 'docker'
            }
          }
        
        stage('Build'){
            steps{
              sh """
                 ls
                 pwd
                 docker ps
                 docker login -u admin -p Harbor12345 localhost:80
                 docker build -t localhost:80/test-project1/projecta:v1 .
                 docker push localhost:80/test-project1/projecta:v1
                 """
                
            }
        }
        stage('Get Kubectl') {
            steps {  
                withKubeConfig([credentialsId: 'adminUser', serverUrl: 'https://10.1.2.58:6443']) {
                    sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'  
                    sh 'chmod u+x ./kubectl'  
                }
            }
          }
        

        stage('Deploy') {
            steps {
                withKubeCredentials([
                    [credentialsId: 'adminUser', serverUrl: 'https://10.1.2.58:6443']
                ]) {
                  sh 'ls'
                  sh './kubectl -n ultron-app apply -f k8/deployment.yaml'
                }
            }
        }
    }
}

