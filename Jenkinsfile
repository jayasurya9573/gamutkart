pipeline {
    agent any

//	tools {
//		jdk 'jdk11'
//	}
//	environment {
//		M2_INSTALL = "/usr/bin/mvn"
//	}

    stages {
        stage('Clone-Repo') {
	    steps {
	        checkout scm
	    }
        }

        stage('Build') {
            steps {
                sh 'mvn install -Dmaven.test.skip=true'
            }
        }
		
        stage('Unit Tests') {
            steps {
                sh 'mvn compiler:testCompile'
                sh 'mvn surefire:test'
                junit 'target/**/*.xml'
            }
        }

        stage('Deployment') {
            steps {
                sh 'sshpass -p "surya" scp target/gamutgurus.war surya@172.17.0.2:/home/surya/apache-tomcat-9.0.78/webapps'
                sh 'sshpass -p "surya" ssh surya@172.17.0.2 "/home/surya/apache-tomcat-9.0.78/bin/startup.sh"'
            }
        }
    }
}
