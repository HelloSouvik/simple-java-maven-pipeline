pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
    	stage ('Initialize') {
        	steps {
        		sh 'mvn -v'
	            sh '''
	              echo "PATH = ${PATH}"
	              echo "M2_HOME = ${M2_HOME}"
	              echo "JAVA_HOME = ${JAVA_HOME}"
	            '''
          	}
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
            	sh "chmod +x -R ${env.WORKSPACE}"
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
