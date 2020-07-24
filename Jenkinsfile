pipeline {
  agent {
    label 'source-builder'
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
    string(description: 'Must be in form $DESTINATION/*****/$releaseXXX/$revision', name: 'JEMALLOC_PATH')
    string(description: 'Must be in form $DESTINATION/*****/$releaseXXX/$revision', name: 'PERL_DBD_PATH')
    string(description: 'Must be in form $DESTINATION/*****/$releaseXXX/$revision', name: 'QPRESS_PATH')
    string(description: 'Must be in form $DESTINATION/*****/$releaseXXX/$revision', name: 'PERCONA_HAPROXY_PATH')
    string(description: 'Must be in form $DESTINATION/*****/$releaseXXX/$revision', name: 'PERCONA_PROXYSQL_PATH')
    string(description: 'Must be in form $DESTINATION/*****/$releaseXXX/$revision', name: 'PERCONA_TOOLKIT_PATH')
    string(description: 'Must be in form $DESTINATION/*****/$releaseXXX/$revision', name: 'PERCONA_XTRABACKUP_PATH')
    string(description: 'Must be in form $DESTINATION/*****/$releaseXXX/$revision', name: 'PERCONA_PXC_PATH')
    string(description: 'Must be in form $DESTINATION/*****/$releaseXXX/$revision', name: 'PERCONA_PS_PATH')
    string(description: 'Must be in form $DESTINATION/*****/$releaseXXX/$revision', name: 'PERCONA_MYSQL_SHELL_PATH')
    string(description: 'Must be in form $DESTINATION/*****/$releaseXXX/$revision', name: 'PERCONA_ORCHESTRATOR_PATH')
    choice(choices: '''PDPS
PDPXC''', description: 'Repository push to', name: 'REPOSITORY')
    string(description: 'Version of repository push to', name: 'REPOSITORY_VERSION')
    booleanParam(defaultValue: false, description: 'if true, will push to both repos', name: 'REPOSITORY_VERSION_MAJOR')
    choice(choices: '''TESTING
RELEASE
EXPERIMENTAL
LABORATORY''', description: 'Separate repository to push to', name: 'COMPONENT')
    booleanParam(defaultValue: false, description: 'Remove lockfile after unsuccessful push for DEB', name: 'REMOVE_LOCKFILE')
    booleanParam(defaultValue: false, description: 'Check to remove sources and binary version if equals pushing', name: 'REMOVE_BEFORE_PUSH')
    booleanParam(defaultValue: false, description: 'Skips push-to-rpm-repository stage', name: 'SKIP_RPM')
    booleanParam(defaultValue: false, description: 'Skips push-to-apt-repository stage', name: 'SKIP_APT')
    booleanParam(defaultValue: false, description: 'Skips sync-repos-to-production stage', name: 'SKIP_SYNC')
    booleanParam(defaultValue: false, description: 'Skips clear-cdn-cache stage', name: 'SKIP_CDN')
    booleanParam(defaultValue: false, description: 'Skips sync-production-downloads stage', name: 'SKIP_PRODUCTION_DOWNLOADS')
    booleanParam(defaultValue: false, description: 'Skips refresh-downloads-area stage', name: 'SKIP_PRODUCTION_REFRESH')
  }
}