node {
  stage "Prepare environment"
  checkout scm

  docker.image('busybox').inside {
    stage "Test"
    sh "exit 1"
  }
}
