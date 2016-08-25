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
    def to = emailextrecipients([
        [$class: 'CulpritsRecipientProvider'],
        [$class: 'DevelopersRecipientProvider'],
        [$class: 'RequesterRecipientProvider']
    ])
    print("to is ${to}")
    to.push('varju@blackboard.com')

    step([$class: 'Mailer', notifyEveryUnstableBuild: true, recipients: to, sendToIndividuals: true])
  }
}

def build() {
  stage "Build"
  sh "exit 1"
}

def test() {
  stage "Test"
  echo "Hello"
}

def publish() {
  stage "Publish"
  echo "Hello"
}
