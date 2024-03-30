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
      NEXUS-USER = 'admin'
      NEXUS-PASS = 'nexus'
      NEXUS-GRP-REPO = 'vprofile-maven-group'
      SNAP-REPO = 'vprofile-snapshot'
      RELEASE-REPO = 'vprofile-release'
      CENTRAL-REPO = 'vprofile-maven-central'
      
    }
    stages {
        stage('Build') {
            steps {
                // Build using settings: we want to download dependencies specified in pom.xml which takes details from settings.xml
                // 
                sh 'mvn -s settings.xml -DskipTests install'
            }

        }
    }
}