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
                 docker login -u admin -p Harbor12345 testharbor.definework.co
                 """
                
            }
        }
        
    }

}
    
