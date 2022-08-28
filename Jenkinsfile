pipeline {
  agent any 
  
  tools {
    maven 'Maven Config'
 }

  stages {

    stage ('Initialize') {
      steps {        
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"                    
           '''
      }
    }
    
    
    stage ('Check-Git-Secrets') {
     steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/kbcasurf/SecureWebApp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    

    stage ('Build') {
      steps { 
        sh 'mvn clean package'
      }
    }


    stage ('Deploy Tomcat') {
      steps {
        sshagent(['TomcatKey']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@18.228.59.13:/home/ubuntu/tomcat/webapps/webapp.war'
              }      
           }       
    }

  } 
}
