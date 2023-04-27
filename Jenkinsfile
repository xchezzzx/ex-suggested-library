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
    tools { maven "Maven-3.6.3" }

    stages {

        stage('Calculate & Set Version') {
            when {
                branch "release/*"
            }
            steps {
                script {
                    
                    newVersionNumber = env.GIT_BRANCH.replaceAll('release/', '')
                    echo "Current version number: ${newVersionNumber}"
                    echo "You are here 1"

                    //checking the version in the pom file
                    def previousVersionNumber = sh(script: "mvn help:evaluate -Dexpression=project.version -q -DforceStdout", returnStdout: true).trim()
                    echo "Existing version number: ${previousVersionNumber}"
                    echo "You are here 2"

                    //sh "mvn versions:compare -DcompareTo=${newVersionNumber}"

                    //setting new version to the pom file
                    sh "mvn versions:set -DnewVersion=${newVersionNumber}"
                    echo "You are here 3"
                    sh "mvn help:evaluate -Dexpression=project.version -q -DforceStdout"
                    echo "You are here 4"

                    // Add the new file to Git
                    sh "git add pom.xml"
                    
                    // Commit the changes with a message
                    sh "git commit -m 'Jenkins triggered build: ${env.BUILD_NUMBER}, set version ${newVersionNumber}'"
                    
                    // Push the changes to the "master" branch
                    sh "git push origin ${env.GIT_BRANCH}"
                }   
            }
        }

        stage('Build & Test') {
            steps {
                script{
                    sh "mvn verify"
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

        // stage('Push changes to git') {
        //     steps {
        //         script {
        //             catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
        //                 withCredentials([usernamePassword(credentialsId: 'example-secure', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
        //                     sh "git add ."
        //                     sh "git commit -m 'Jenkins triggered build: ${env.BUILD_NUMBER}'"
        //                     sh "git push -u origin ${env.GIT_BRANCH}"
        //                 }
        //             }
        //         }
        //     }
        // }
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