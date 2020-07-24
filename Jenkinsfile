pipeline {
  agent {
    label 'docker'
  }
  stages {
    stage('Get Sources') {
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
    string(defaultValue: 'https://github.com/percona/percona-server.git', description: 'github repository for build', name: 'GIT_REPO')
    string(defaultValue: '8.0', description: 'Tag/Branch for percona-server repository', name: 'BRANCH')
    string(defaultValue: '0', description: 'PerconaFT repository', name: ' 	PERCONAFT_REPO')
    string(defaultValue: 'master', description: 'Tag/Branch for PerconaFT repository', name: '')
    string(defaultValue: '0', description: 'TokuBackup repository', name: 'TOKUBACKUP_REPO')
    string(defaultValue: 'master', description: 'Tag/Branch for TokuBackup repository', name: 'TOKUBACKUP_BRANCH')
    string(defaultValue: '1', description: 'RPM version', name: 'RPM_RELEASE')
    string(defaultValue: '1', description: 'DEB version', name: 'DEB_RELEASE')
    choice(choices: '''NO
YES''', description: 'Is this build for release?', name: 'RELEASE_BUILD')
    choice(choices: '''laboratory
testing
experimental
release''', description: 'Repo component to push packages to', name: 'COMPONENT')
  }
}