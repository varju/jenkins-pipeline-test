#!/usr/bin/env groovy

// Required plugins:
// - Pipeline (2.2)
// - Pipeline: Multibranch (2.8)
// - CloudBees Docker Pipeline (1.7)
// - Email Extension Plugin (2.47)

stage 'Setup'
node {
  emailHandler {
    wrap([$class: 'TimestamperBuildWrapper']) {
      docker.image('busybox').inside("--volume ${env.MESOS_SANDBOX}:/data") {
        build()
        test()
        publish()
      }
    }
  }
}

def emailHandler(Closure block) {
  try {
    block()
  } catch (error) {
    currentBuild.result = 'FAILURE'
    throw error
  } finally {
    def to = emailextrecipients([
        [$class: 'CulpritsRecipientProvider'],   // committers since last successful build
        [$class: 'DevelopersRecipientProvider'], // committers since previous build
        [$class: 'RequesterRecipientProvider']   // user who triggered build (if manually built)
    ])

    print("to was $to")
    if (to == null) {
      to = 'varju@blackboard.com'
    }
    else {
      to += ',varju@blackboard.com'
    }
    print("to is $to")

    // TODO: This does not detect committers for the first build of a feature branch
    // TODO: This doesn't appear to be sending an email when failing builds recover
    step([$class: 'Mailer', notifyEveryUnstableBuild: true, recipients: to, sendToIndividuals: true])
  }
}


//
// Build steps
//

def build() {
  stage "Build"

  sh "env"
  sh "mount"
  sh "ls -aFl /data"
  sh "touch /data/asdf.${env.BUILD_ID}"
  sh "rm -f /data/asdf*"
  // sh "df -h"

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
