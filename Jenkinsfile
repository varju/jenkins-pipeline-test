#!/usr/bin/env groovy

node {
  try {
    // stage "Prepare environment"
    // checkout scm

    // docker.image('busybox').inside {
    stage "Test"
    sh "exit 0"
  // }
  } catch (error) {
    // Send an email to the developers who have committed recently, and to the person who requested the build
    def to = emailextrecipients([
        [$class: 'CulpritsRecipientProvider'],
        [$class: 'DevelopersRecipientProvider'],
        [$class: 'RequesterRecipientProvider']
    ])
    if (to != null) {
      echo "Email recipients: $to"
    }
    if (to != null && !to.isEmpty()) {
        mail to: to,
             subject: "[JENKINS] ${env.JOB_NAME} failed",
             body: "Build failed (see ${env.BUILD_URL}): ${error}"
    }
    throw error
  }
}
