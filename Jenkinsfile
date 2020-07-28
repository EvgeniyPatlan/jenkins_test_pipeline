pipeline {
  agent {
    label 'docker'
  }
  stages {
    stage('Prepare') {
      steps {
        git(branch: BRANCH, url: GIT_REPO)
        sh '''
                    # sudo is needed for better node recovery after compilation failure
                    # if building failed on compilation stage directory will have files owned by docker user
                    sudo git reset --hard
                    sudo git clean -xdf
                   
                    git rev-parse --short HEAD > shortCommit
                    ##need to add version to product name
                    echo "UPLOAD/experimental/${JOB_NAME}/percona-server-8.0/Percona-Server-8.0/${GIT_BRANCH}/$(cat shortCommit)/${BUILD_NUMBER}" > uploadPath
                '''
        stash(includes: 'uploadPath', name: 'uploadPath')
        archiveArtifacts 'shortCommit'
        archiveArtifacts 'uploadPath'
      }
    }

    stage('Build percona-server source') {
      steps {
        sh '''sg docker -c "
            ls -la
        "'''
        stash(name: 'source.tarball', includes: 'results/source_tarball/*.tar.*')
      }
    }

  }
  options {
    skipDefaultCheckout()
    disableConcurrentBuilds()
  }
  parameters {
    string(defaultValue: 'https://github.com/EvgeniyPatlan/percona-server.git', description: 'github repository for build', name: 'GIT_REPO')
    string(defaultValue: '8.0', description: 'Tag/Branch for percona-server repository', name: 'BRANCH')
    string(defaultValue: '0', description: 'PerconaFT repository', name: '     PERCONAFT_REPO')
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