pipeline {
	agent none
	stages {
		stage('Integration UI Test') {
			parallel {
				stage('Deploy') {
					agent any
					steps {
						sh 'chmod +x ./jenkins/scripts/deploy.sh'
						input message: 'Finished using the web site? (Click "Proceed" to continue)'
						sh 'chmod +x ./jenkins/scripts/kill.sh'
					}
				}
				stage('Headless Browser Test') {
					steps {
						sh 'docker run -d -v /root/m2:/root/.m2 maven:latest'
						sh 'mvn -B -DskipTests clean package'
						sh 'mvn test'
					}
					post {
						always {
							junit 'target/surefire-reports/*.xml'
						}
					}
				}
			}
		}
	}
}
