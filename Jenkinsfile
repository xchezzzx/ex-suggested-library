// Suggested-library-Java
// Write a Jenkinsfile that performs the following steps:
// Calculate & Set Version: This stage is run when the branch name matches the pattern "release/*". It calculates the next version number for the release, sets the version number in the project's pom.xml file, and commits the change to the source code repository.
// (The incrementation rules for the Java exercise you're working on involve calculating the next version number for the release. This is done by using the major and minor version numbers from the branch name and incrementing the patch version number.
// For example, if you push a release/1.0.4 branch but the pom file has version 1.0.1, you would need to update the pom file to reflect the new version number.
// This can be done using the Maven "versions:set" command, which sets the new version number specified by the "newVersion" parameter...)
// Build & Test: This stage builds and tests the project.
// Add a slack notification
// which informs if the build succeeded or failed
// Bonus:
// Add Code analysis with 'checkstyle'

pipeline {
    agent any
    tools { maven 'Maven 3.9.1' }

    stages {
        stage('wtf') {
            steps {
                script {
                    echo env.GIT_BRANCH
                }
            }
        }

        stage('Calculate & Set Version') {
            when {
                branch 'release/'
            }
            steps {
                // Calculate next version number
                script {
                    echo 'step 2'
                }   
            }
        }

        stage('Build  Test') {
            steps {
                script{
                    echo 'step 2'
                }
            }

        }

        stage('Code analysis') {
            steps {
                script {
                echo 'step 3'
                }
            }
        }
    }

    post {
        success {
            slackSend(
                channel: '#ex-suggested-library',
                color: 'good',
                message: "The ${currentBuild.fullDisplayName} succeeded!",
                token: 'slack-token'
            )
        }
        failure {
            slackSend(
                channel: '#ex-suggested-library',
                color: 'danger',
                message: "The ${currentBuild.fullDisplayName} failed!",
                token: 'slack-token'
            )
        }
    }
}