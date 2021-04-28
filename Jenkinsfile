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
        
        
        stage('Build'){
            steps{
              sh """
                 ls
                 pwd
                 docker container ls
                 docker login -u admin -p Harbor12345 10.1.2.58:30002
                 docker build -t 10.1.2.58:30002/test-project1/projecta:v1 .
                 docker push 10.1.2.58:30002/test-project1/projecta:v1
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

