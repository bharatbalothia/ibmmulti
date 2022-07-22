pipeline {
    agent any

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
        stage('ansible') {
            steps {
                git 'https://github.com/bharatbalothia/ibmjulytomcat.git'
                ansiblePlaybook credentialsId: 'ansible-id', disableHostKeyChecking: true, inventory: 'dev.inv', playbook: 'tomcat.yml'
            }
        }
    }
}

