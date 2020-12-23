pipeline{
	
	tools{

		jdk 'myjava'
		maven 'my-maven'
	}
	agent none

	stages{

		stage('complile') {

			agent any
			steps{
				sh 'mvn compile'
			}
		}

		stage('codereview'){

			agent any
			steps{
				sh 'mvn pmd:pmd'
			}
			//post{
			//	always{

			//		pmd pattern: '**/target/pmd.xml'
			//	}
			//}
		}

		stage('unit-test'){

			agent any
			steps{
				sh 'mvn test'
			}
		}

		stage('metric-check'){

			agent any
			steps{

				sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
			}
			post{
				always{
					cobertura coberturaReportFile: 'target/site/cobertura/coverage.xml'
				}
			}
		}
		
		stage('package'){

			agent any
			steps{
				sh 'mvn package'
			}
		}

		stage('deploy'){

                        agent any
                        steps{
                                sh 'rm -rf dockerJenkins'
                                sh 'mkdir dockerJenkins'
                                sh 'cd dockerJenkins'
                                sh 'cp /var/lib/jenkins/workspace/package/target/addressbook.war .'
                                sh 'touch Dockerfile'
                                sh 'echo "FROM tomcat" >> Dockerfile'
                                sh 'echo "ADD addressbook.war /usr/local/tomcat/webapps" >> Dockerfile'
                                sh 'echo "CMD ["\catalina.sh"\, "\run"\]" >> Dockerfile'
                                sh 'echo "EXPOSE 8080" >> Dockerfile'
                                sh 'sudo docker build -t myimage:$BUILD_NUMBER .'
                                sh 'sudo docker run -itd -P myimage:$BUILD_NUMBER'

                        }
                }

	}
}
