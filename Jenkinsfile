pipeline {
  agent {
    docker {
      image 'ubuntu:xenial'
    }

  }
  stages {
    stage('Get Sources') {
      agent {
        docker {
          image 'centos/7'
        }

      }
      steps {
        sh 'echo "Test pipeline"'
      }
    }

  }
  options {
    skipDefaultCheckout()
    disableConcurrentBuilds()
  }
  parameters {
    string(defaultValue: '8.0', description: 'Tag/Branch for percona-server repository', name: 'GIT_BRANCH')
    string(defaultValue: '1', description: 'REVISION for release build only', name: 'REVISION')
    choice(choices: '''NO
YES''', description: 'Is this build for release?', name: 'RELEASE_BUILD')
    choice(choices: '''laboratory
testing
experimental
release''', description: 'Repo component to push packages to', name: 'COMPONENT')
  }
}