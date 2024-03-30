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
      
    }
    stages {
        stage('Build') {
            steps {
                // Build using settings: we want to download dependencies specified in pom.xml which takes details from settings.xml
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now Archiving..."
                    archiveArticacts artifacts: '**/*.war'
                }
            }
        }
        stage('Test') {
            steps {
                //unit tests
                sh 'mvn test'
            }
        }
        stage('Checkstyle Analysis') {
            steps {
                //code analysis tool, vulnurabilities
                sh 'mvn checkstyle:checkstyle'
            }
        }

    }
}