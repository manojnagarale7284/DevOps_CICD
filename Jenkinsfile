pipeline{
   agent any
   
   tools {
      jdk 'jdk17'
	  maven 'maven3'
	  }
	environment {
		SCANNER_HOME =  tool 'sonar-scanner'
            }
	stages {
        stage("GIT CHECKOUT") {
	       steps {
		    git branch: 'main', changelog: false,  poll:false, url: 'https://github.com/manojnagarale7284/DevOps_CICD.git'
		    }
		   }
   
        stage ("Compile Code") {
			steps {
			   sh "mvn clean compile"
			   }
			 }

		stage("SONARQUBE Analysis") {
			steps {
			   withSonarQubeEnv('sonarserver') {
				  sh ''' $SCANNER_HOME/bin/sonar-scanner -X  -Dsonar.projectName=DevOps-CICD \
						 -Dsonar.javaBinaries=. \
                                                 -Dsonar.host.url=http:http://139.59.44.164:9000/
                                                 -Dsonar.sonar.login='3e1e60ae2feffbabaaf3722c04b69016143ac723'
						 -Dsonar.projectKey=DevOps-CICD '''
                        }						 
	             } 
            }
		stage("TRIVY SCAN") {
			steps {
				sh "trivy fs --secirity-checks viln,config /var/lib/jenkins/workspace/CICD"
				}
			}

        stage("CODE BUILD") {
		   steps {
		      sh 'mvn clean install'
			  }
			}  
}
}
