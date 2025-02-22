pipeline {
    agent any
    tools {
        maven "MAVEN3.9.9"
        jdk "JDK17"
    }
    
    environment {
        SNAP_REPO = 'vprofile-snapshot'
		NEXUS_USER = 'admin'
		NEXUS_PASS = 'Admin#1234'
		RELEASE_REPO = 'vprofile-release'
		CENTRAL_REPO = 'vpro-maven-central'
		NEXUSIP = '192.168.101.124'
		NEXUSPORT = '8081'
		NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
    }

    stages {
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }
    }
    stage('Test'){
         steps {
           sh 'mvn -s settings.xml test'
         }
    }
    stage('Checkstyle Analysis'){
         steps {
           sh 'mvn -s settings.xml checkstyle:checkstyle'
         }
    }
    stage('Sonar Analysis'){
         steps {
            withSonarQubeEnv("${SONARSERVER}){
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
               -Dsonar.projectName=vprofile \
               -Dsonar.projectVersion=1.0 \
               -Dsonar.sources=src/ \
               -Dsonar.java.binaries=target/target-classes/com/visualpathit/account/controllerTest \
               -Dsonar.junit.reportsPath=target/surefire-reports/ \
               -Dsonar.jacoco.reportsPath=target/jacoco.exec \
               -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
              }
          }
    }
       
}
