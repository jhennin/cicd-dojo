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
                stage('SonarQube analysis') {
                        steps {
                                withSonarQubeEnv('sonar') {
                                        // requires SonarQube Scanner for Maven 3.2+
                                        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.6.0.1398:sonar -Dsonar.host.url=$SONAR_HOST_URL  -Dsonar.login=6f4c312e65461651d571e2db6dba5db22e2630ba -Dsonar.projectKey=cicd-dojo -Dsonar.organization=jhennin-github'
                                }
                        }
                }
                stage("Quality Gate") {
                        steps {
                                timeout(time: 1, unit: 'HOURS') {
                                        // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                                        // true = set pipeline to UNSTABLE, false = don't
                                        waitForQualityGate abortPipeline: true
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
