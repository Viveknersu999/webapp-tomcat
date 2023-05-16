pipeline {
    agent any
    stages {
        stage('Welcome') { 
            steps { 
               echo 'This is a DevSecOps pipeline created by AWSTechGuide' 
            }
        }
        stage('Pull Sources') {
            steps {
             git url: 'https://github.com/awstechguide/spring-webapp.git'
            }
         }
        
        stage('Check-Git-Secrets') {
            steps {
             sh 'rm trufflehog* || true'
             sh 'docker pull gesellix/trufflehog'
	     sh 'docker run -t gesellix/trufflehog --json https://github.com/awstechguide/spring-webapp.git > trufflehog'
	     sh 'cat trufflehog'
            }
         }
        
/*	    
        stage('SCA') {
            steps {
                echo 'Initiating  Source Composition Analysis by OWASP' 
                sh 'rm owasp* || true'
                sh 'wget "https://raw.githubusercontent.com/awstechguide/spring-webapp/master/owasp-dependency-checker.sh"'
                sh 'chmod +x owasp-dependency-checker.sh'
                sh 'bash owasp-dependency-checker.sh'
                sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
            }
        }
	    */
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

	    
	stage ('Deploy') {
          steps {
	sshagent(['deployuser']) {
	    sh "scp -o StrictHostKeyChecking=no target/webapptest.war ubuntu@54.92.220.219:/usr/local/tomcat/webapps/webapptest.war"
	    }
	  }
	}
        
    stage ('DAST') {
      stage ('Deploy') {
          steps {
sh "cd /var/lib/jenkins/workspace/webapp-vivek/target/"
sh "aws s3 cp webapptest.war s3://webapp-vivek/" 
//sh "scp -o StrictHostKeyChecking=no Webapp-Pipeline/target/webapptest.war ec2-user@13.233.106.41:/home/ec2-user/apache-tomcat-9.0.75/webapps/webapptest.war"
                }
   }  
      }
    }    	    
	    
    }
}
