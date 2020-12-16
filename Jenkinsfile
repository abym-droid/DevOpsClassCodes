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
			//		pmd pattern:'target/pmd.xml'
			//	}
			
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

			agent {label 'linux_slave'}
			steps{
				sh 'mvn package'
			}
		}
	}
}
