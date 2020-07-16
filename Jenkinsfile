pipeline {
  agent {
    label 'source-builder'
  }
  stages {
    stage('push-to-repository') {
      parallel {
        stage('push-to-rpm-repository') {
          steps {
            withCredentials(bindings: [string(credentialsId: 'SIGN_PASSWORD', variable: 'SIGN_PASSWORD')]) {
              withCredentials(bindings: [sshUserPrivateKey(credentialsId: 'repo.ci.percona.com', keyFileVariable: 'KEY_PATH', passphraseVariable: '', usernameVariable: 'USER')]) {
                sh '''for var in "$(printenv | grep _PATH | sed \'s/KEY_PATH.*//\')"; do
                                echo "$var" >> args_pipeline
                            done
                            
                            echo "REPOSITORY=${REPOSITORY}" >> args_pipeline
                            echo "REPOSITORY_VERSION=${REPOSITORY_VERSION}" >> args_pipeline
                            echo "REPOSITORY_VERSION_MAJOR=${REPOSITORY_VERSION_MAJOR}" >> args_pipeline
                            echo "COMPONENT=${COMPONENT}" >> args_pipeline
                            echo "REMOVE_LOCKFILE=${REMOVE_LOCKFILE}" >> args_pipeline
                            echo "REMOVE_BEFORE_PUSH=${REMOVE_BEFORE_PUSH}" >> args_pipeline
                            echo "$(awk \'{$1="export" OFS $1} 1\' args_pipeline)" > args_pipeline
                            cat args_pipeline
                            rsync -av -e "ssh -o StrictHostKeyChecking=no -i \\$KEY_PATH" args_pipeline \\$USER@repo.ci.percona.com:/tmp/args_pipeline
                            ssh -o StrictHostKeyChecking=no -i \\$KEY_PATH \\$USER@repo.ci.percona.com \' \\
                                bash -x /tmp/args_pipeline
                                wget https://raw.githubusercontent.com/EvgeniyPatlan/jenkins-pipelines/DISTMYSQL-36/ps-pxc-dist/rpm_release.sh -O rpm_release.sh
                                bash -xe rpm_release.sh
                            \''''
              }

            }

          }
        }

        stage('push-to-deb-repository') {
          steps {
            withCredentials(bindings: [string(credentialsId: 'SIGN_PASSWORD', variable: 'SIGN_PASSWORD')]) {
              withCredentials(bindings: [sshUserPrivateKey(credentialsId: 'repo.ci.percona.com', keyFileVariable: 'KEY_PATH', passphraseVariable: '', usernameVariable: 'USER')]) {
                sh '''for var in "\\$(printenv | grep _PATH | sed \'s/KEY_PATH.*//\')"; do
                                echo "\\$var" >> args_pipeline
                            done

                            echo "REPOSITORY=\\${REPOSITORY}" >> args_pipeline
                            echo "REPOSITORY_VERSION=\\${REPOSITORY_VERSION}" >> args_pipeline
                            echo "REPOSITORY_VERSION_MAJOR=\\${REPOSITORY_VERSION_MAJOR}" >> args_pipeline
                            echo "COMPONENT=\\${COMPONENT}" >> args_pipeline
                            echo "REMOVE_LOCKFILE=\\${REMOVE_LOCKFILE}" >> args_pipeline
                            echo "REMOVE_BEFORE_PUSH=\\${REMOVE_BEFORE_PUSH}" >> args_pipeline
                            echo "\\$(awk \'{\\$1="export" OFS \\$1} 1\' args_pipeline)" > args_pipeline
                            rsync -av -e "ssh -o StrictHostKeyChecking=no -i \\$KEY_PATH" args_pipeline \\$USER@repo.ci.percona.com:/tmp/args_pipeline
                            ssh -o StrictHostKeyChecking=no -i \\$KEY_PATH \\$USER@repo.ci.percona.com \' \\
                                bash -x /tmp/args_pipeline
                                wget https://raw.githubusercontent.com/EvgeniyPatlan/jenkins-pipelines/DISTMYSQL-36/ps-pxc-dist/apt_release.sh -O apt_release.sh
                                bash -xe apt_release.sh
                            \''''
              }

            }

          }
        }

      }
    }

    stage('sync-repos-to-production') {
      steps {
        withCredentials(bindings: [sshUserPrivateKey(credentialsId: 'repo.ci.percona.com', keyFileVariable: 'KEY_PATH', passphraseVariable: '', usernameVariable: 'USER')]) {
          sh '''for var in "$(printenv | grep _PATH | sed \'s/KEY_PATH.*//\')"; do
                                echo "$var" >> args_pipeline
                            done

                            echo "REPOSITORY=${REPOSITORY}" >> args_pipeline
                            echo "REPOSITORY_VERSION=${REPOSITORY_VERSION}" >> args_pipeline
                            echo "REPOSITORY_VERSION_MAJOR=${REPOSITORY_VERSION_MAJOR}" >> args_pipeline
                            echo "COMPONENT=${COMPONENT}" >> args_pipeline
                            echo "REMOVE_LOCKFILE=${REMOVE_LOCKFILE}" >> args_pipeline
                            echo "REMOVE_BEFORE_PUSH=${REMOVE_BEFORE_PUSH}" >> args_pipeline
                            echo "$(awk \'{$1="export" OFS $1} 1\' args_pipeline)" > args_pipeline
                            rsync -av -e "ssh -o StrictHostKeyChecking=no -i \\$KEY_PATH" args_pipeline \\$USER@repo.ci.percona.com:/tmp/args_pipeline
                            ssh -o StrictHostKeyChecking=no -i \\$KEY_PATH \\$USER@repo.ci.percona.com \' \\
                                cd /srv/repo-copy
                                REPO=$(echo ${REPOSITORY} | tr \'[:upper:]\' \'[:lower:]\' )
                                RSYNC_TRANSFER_OPTS="-avt  --delete --delete-excluded --delete-after --progress"
                                rsync ${RSYNC_TRANSFER_OPTS} --exclude=*.sh --exclude=*.bak /srv/repo-copy/${REPO}/* 10.10.9.209:/www/repo.percona.com/htdocs/${REPO}/'''
        }

      }
    }

    stage('clear-cdn-cache') {
      steps {
        withCredentials(bindings: [sshUserPrivateKey(credentialsId: 'repo.ci.percona.com', keyFileVariable: 'KEY_PATH', passphraseVariable: '', usernameVariable: 'USER')]) {
          sh '''ssh -o StrictHostKeyChecking=no -i \\$KEY_PATH \\$USER@repo.ci.percona.com \' \\
                            bash +x /usr/local/bin/clear_cdn_cache.sh'''
        }

      }
    }

    stage('sync-production-downloads') {
      steps {
        withCredentials(bindings: [sshUserPrivateKey(credentialsId: 'repo.ci.percona.com', keyFileVariable: 'KEY_PATH', passphraseVariable: '', usernameVariable: 'USER')]) {
          sh '''if [[ "${COMPONENT}" == "RELEASE" ]]; then
                            for var in "$(printenv | grep _PATH | sed \'s/KEY_PATH.*//\')"; do
                                echo "$var" >> args_pipeline
                            done
                            echo "REPOSITORY=${REPOSITORY}" >> args_pipeline
                            echo "REPOSITORY_VERSION=${REPOSITORY_VERSION}" >> args_pipeline
                            echo "REPOSITORY_VERSION_MAJOR=${REPOSITORY_VERSION_MAJOR}" >> args_pipeline
                            echo "COMPONENT=${COMPONENT}" >> args_pipeline
                            echo "REMOVE_LOCKFILE=${REMOVE_LOCKFILE}" >> args_pipeline
                            echo "REMOVE_BEFORE_PUSH=${REMOVE_BEFORE_PUSH}" >> args_pipeline
                            echo "$(awk \'{$1="export" OFS $1} 1\' args_pipeline)" > args_pipeline
                            rsync -av -e "ssh -o StrictHostKeyChecking=no -i \\$KEY_PATH" args_pipeline \\$USER@repo.ci.percona.com:/tmp/args_pipeline
                            ssh -o StrictHostKeyChecking=no -i \\$KEY_PATH \\$USER@repo.ci.percona.com \' \\
                                bash -x /tmp/args_pipeline
                                wget https://raw.githubusercontent.com/EvgeniyPatlan/jenkins-pipelines/DISTMYSQL-36/ps-pxc-dist/downloads_release.sh -O downloads_release.sh
                                bash -xe downloads_release.sh
                            \'
                        fi'''
        }

      }
    }

    stage('refresh-downloads-area') {
      steps {
        sh '''if [[ "${COMPONENT}" == "RELEASE" ]]; then
                        sudo apt-get update
                        sudo apt-get -y install curl
                        if [[ "${COMPONENT}" == "RELEASE" ]]; then
                            wget https://www.percona.com/admin/config/percona/percona_downloads/crawl_directory
                        fi
                    fi'''
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