pipeline {
  agent any
  
  stages {
    stage('Initialize'){
            steps {
                script {
                    def dockerHome = tool 'docker-root'
                    env.PATH = "${dockerHome}/bin:${env.PATH}"   
                    
                }
    
            }
        }
    
    stage('Build Image') {
      steps {
        withEnv(['PATH+EXTRA=/usr/sbin:/usr/bin:/sbin:/bin']) { 
          sh "docker pull hello-world"
          
        }
      }
    }  
    stage('Push Image') {
      steps {
         withEnv(['PATH+EXTRA=/usr/sbin:/usr/bin:/sbin:/bin']) { 
           sh """
              docker login -u admin -p Harbor12345 10.1.2.58:80
              docker image tag hello-world 10.1.2.58:80/test-project1/testimage2:v2
              docker push 10.1.2.58:80/test-project1/testimage2:v2
             """
        
     }
    }
   }
  }
}  
