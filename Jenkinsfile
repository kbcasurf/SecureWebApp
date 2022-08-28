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

    
    
    stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://github.com/kbcasurf/SecureWebApp/blob/main/owasp-dependency-check.sh"'
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash ./owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'        
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
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.94.255.145:/home/ubuntu/tomcat/webapps/webapp.war'
              }      
           }       
    }

  } 
}
