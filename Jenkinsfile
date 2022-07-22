pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "m3"
    }

    stages {
        stage('Code from SCM') {
            steps {
                // Get some code from a GitHub repository
                git credentialsId: 'git-token', url: 'git@github.com:bharatbalothia/jenkins.git'

            }
        }
        stage('Mvn build') {
            steps {
                // Get some code from a GitHub repository
                sh 'mvn -B -DskipTests clean package'

            }
        }
        stage('Unit Test') {
            steps {
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
            }
        }
        stage('Checkstyle Test') {
            steps {
                // Get some code from a GitHub repository
                sh 'mvn checkstyle:checkstyle'
                recordIssues(tools: [checkStyle(pattern: '**/checkstyle-result.xml')])

            }
        }
        stage('code coverage') {
            steps {
                // Get some code from a GitHub repository
                jacoco()

            }
        }
        stage ('Nexus upload')  {
          steps {
          nexusArtifactUploader(
          nexusVersion: 'nexus3',
          protocol: 'http',
          nexusUrl: '44.209.130.198:8081',
          groupId: 'websocket-demo',
          version: '0.0.1-SNAPSHOT',
          repository: 'maven-snapshots',
          credentialsId: 'nexus-cred',
          artifacts: [
            [artifactId: 'websocket-demo',
             classifier: '',
             file: 'target/websocket-demo-0.0.1-SNAPSHOT.jar',
             type: 'jar']
        ]
        )
          }
        }

        //stage('sonar testing') {
          //  when {
            //    expression {choice  == 3}
            //}
            //steps {
                // Get some code from a GitHub repository
             //   sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=chatapp -Dsonar.host.url=http://44.209.130.198:9000 -Dsonar.login=sqp_5a025935312a61ce25f0280e6ff44fe4222fa29b'

            //}
        //}
    }
}
