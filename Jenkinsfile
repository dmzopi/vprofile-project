pipeline {
    agent any
    tools {
        // put here tools names as specified in Jenkins config
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    // Specify env variables used in settings.xml and pom.xml
    environment {
      
      NEXUSIP = 'nexus.pokhrime.vas'
      NEXUSPORT = '8081'
      NEXUS_LOGIN = 'nexuslogin' //as specified in jenkins config
      NEXUS_USER = 'admin'
      NEXUS_PASS = 'nexus'
      NEXUS_GRP_REPO = 'vprofile-maven-group'
      SNAP_REPO = 'vprofile-snapshot'
      RELEASE_REPO = 'vprofile-release'
      CENTRAL_REPO = 'vprofile-maven-central'
      SONARSERVER = 'sonarserver'
      SONARSCANNER = 'sonarscanner'
      
    }
    stages {
        stage('Build') {
            steps {
                // Build using settings: we want to download dependencies specified in pom.xml which takes details from settings.xml
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now Archiving..." // Archived files will be accessible from the Jenkins webpage.
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Test') {
            steps {
                //unit tests: execute src/test/* -> put results to target/surefire-report
                sh 'mvn test'
            }
        }
        stage('Checkstyle Analysis') {
            steps {
                //code analysis tool, vulnurabilities. Put result to target/checkstyle-result.xml etc.
                sh 'mvn checkstyle:checkstyle'
            }
        }
        // Get all report and ket SonarQube analyse it
        stage('Sonar Analysis') {
    environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps {
               withSonarQubeEnv("${SONARSERVER}") {
                   sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
              }
            }
        }


    }
}