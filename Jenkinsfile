pipeline {
        agent any
        tools {
		maven 'maven3'
		jdk 'jdk8'
	}
	stages {
                stage ('build') {
                        steps {
                                sh 'mvn clean install'
                        }
                        post {
                                success {
                                        junit '**/target/surefire-reports/TEST-*.xml'
                                        archiveArtifacts 'target/*.jar'
                                }
                        }
                }
                stage('upload nexus') {
                        steps {
                                sh 'mvn deploy'
                        }
                }
        }
}
