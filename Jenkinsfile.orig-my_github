pipeline {
    agent { label 'linux' }
    stages {
        stage ('Checkout') {
          steps {
            git 'https://github.com/bobant/spring-petclinic.git'
          }
        }
        stage('Build') {
	    agent { docker 'maven:3.5-alpine' }
            steps {
                sh 'mvn clean package'
                junit '**/target/surefire-reports/TEST-*.xml'
		archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Deploy') {
	    steps {
		input 'Do you approve the deployment?'
		sh 'scp target/*.jar jenkins@app1:/opt/pet'
		sh "ssh jenkins@app1 'nohup java -jar /opt/pet/spring-petclinic-2.0.0.BUILD-SNAPSHOT.jar &'"
            }
	}

    }
}
