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
                                        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.6.0.1398:sonar -Dsonar.host.url=$SONAR_HOST_URL  -Dsonar.login=ba68952245ce04f276b04fdcf4ddc6f93a5bb2d2 -Dsonar.projectKey=dcdojo -Dsonar.organization=amineoualialami-github'
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
