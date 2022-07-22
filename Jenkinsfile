pipeline {
    agent any
    environment {
        registry = '44.209.130.198:8085/chatapp'
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
                git credentialsId: 'git-token', url: 'git@github.com:bharatbalothia/jenkins.git'

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
                    withDockerRegistry(credentialsId: 'nexus-cred', url: 'http://44.209.130.198:8085') {
                        dockerImage.push()
                       }
                }
            }
        }
    }
}

