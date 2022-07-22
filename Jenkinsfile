pipeline {
    agent any
    environment {
        registry = '54.235.201.140:8085/chatapp'
        registryCredentails = 'nexus-cred'
        dockerImage = ''
    }
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "m3"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git credentialsId: 'git-token-2', url: 'git@github.com:gopal1409/ibmchatapp.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    dockerImage = docker.build registry + "$BUILD_NUMBER"
                }
            }
        }
        stage('push image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'nexus-cred', url: 'http://54.235.201.140:8085') {
                        dockerImage.push()
                       }
                }
            }
        }
    }
}

