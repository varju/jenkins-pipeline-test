#!/usr/bin/env groovy

// Required plugins:
// - Pipeline (2.2)
// - Pipeline: Multibranch (2.8)
// - CloudBees Docker Pipeline (1.7)
// - Email Extension Plugin (2.47)

node {
  try {
    wrap([$class: 'TimestamperBuildWrapper']) {
      docker.image('busybox').inside {
        build()
        test()
        publish()
      }
    }
  } catch (error) {
    currentBuild.result = 'FAILURE'
    throw error
  } finally {
    // def to = emailextrecipients([
    //     [$class: 'CulpritsRecipientProvider'],   // committers since last successful build
    //     [$class: 'DevelopersRecipientProvider'], // committers since previous build
    //     [$class: 'RequesterRecipientProvider']   // user who triggered build (if manually built)
    // ])
    // print("to was ${to}")
    // if (to == null) {
    //   to = 'varju@blackboard.com'
    // }
    // else {
    //   to += ',varju@blackboard.com'
    // }
    // print("to is ${to}")

    step([$class: 'Mailer', notifyEveryUnstableBuild: true, recipients: 'varju@blackboard.com', sendToIndividuals: true])
  }
}

def build() {
  stage "Build"
  sh "exit 0"
}

def test() {
  stage "Test"
  echo "Hello"
}

def publish() {
  stage "Publish"
  echo "Hello"
}
