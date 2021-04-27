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
        
           stage('Get ctr') {
            steps {  
                sh 'wget https://github.com/containerd/containerd/releases/download/v1.4.4/containerd-1.4.4-linux-amd64.tar.gz'
                sh 'tar xvf containerd-1.4.4-linux-amd64.tar.gz'
                sh './bin/ctr image pull docker.io/library/hello-world:latest'

            }
          }
        
        stage('Build'){
            steps{
              sh """
                 ls
                 pwd
                 ./docker/docker container ls
                 ./docker/docker login -u admin -p Harbor12345 localhost:80
                 ./docker/docker build -t localhost:80/test-project1/projecta:v1 .
                 ./docker/docker push localhost:80/test-project1/projecta:v1
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

