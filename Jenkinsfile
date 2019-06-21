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
                stage('Hello') {
                        steps {
                                echo 'Hello DC team this is your first pipeline.'
                        }
                }
        }
}
