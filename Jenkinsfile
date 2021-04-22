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
                 docker ps
                 docker login -u admin -p Harbor12345 localhost:80
                 docker build -t localhost:80/test-project1/projecta:v1 .
                 docker push localhost:80/test-project1/projecta:v1
                 """
                
            }
        }
        stage('List pods') {
            withKubeConfig([credentialsId: 'adminUser', serverUrl: 'https://10.1.2.58:6443']) {
                sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'  
                sh 'chmod u+x ./kubectl'  
                sh './kubectl get pods'
            }
          }

        stage('Deploy') {
            steps {
                withKubeCredentials([
                    [credentialsId: 'adminUser', serverUrl: 'https://10.1.2.58:6443']
                ]) {
                  sh './kubectl apply -f k8/deployment.yaml'
                }
            }
        }
    }
}

