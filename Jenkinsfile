node {
  stage "Prepare environment"
  checkout scm

  docker.image('busybox').inside {
    stage "Test"
    sh "echo hello"
  }
}
