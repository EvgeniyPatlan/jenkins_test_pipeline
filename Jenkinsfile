pipeline {
  agent {
    label 'docker'
  }
  stages {
    stage('Build percona-server source') {
      steps {
        sh '''"""
                                                                                                                                        set -o xtrace
                                                                                                                                        rm -rf test
                                                                                                                                        mkdir test
                                                                                                                                        wget https://raw.githubusercontent.com/EvgeniyPatlan/percona-server/8.0/build-ps/percona-server-8.0_builder.sh -O percona-server_builder.sh
                                                                                                                                        sed -i \'s:sudo::g\' percona-server_builder.sh
                                                                                                                                        pwd -P
                                                                                                                                        ls -laR
                                                                                                                                        export build_dir=\\$(pwd -P)
                                                                                                                                        docker run -u root -v \\${build_dir}:\\${build_dir} ubuntu:xenial sh -c "
                                                                                                                                        set -o xtrace
                                                                                                                                        cd \\${build_dir}
                                                                                                                                        bash -x ./percona-server_builder.sh --builddir=\\${build_dir}/test --install_deps=1; \\
                                                                                                                                        apt-get -y install git; \\
                                                                                                                                        bash -x ./percona-server_builder.sh --builddir=\\${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --get_sources=1"
                                                                                                                                    """'''
        stash(includes: 'source_tarball/percona-server*.tar.gz', name: 'source_tarball', useDefaultExcludes: true)
      }
    }

    stage('Build PS generic source rpm') {
      steps {
        unstash 'test.txt'
        sh """
                                                                                                                                                set -o xtrace
                                                                                                                                                rm -rf test
                                                                                                                                                mkdir test
                                                                                                                                                wget https://raw.githubusercontent.com/EvgeniyPatlan/percona-server/8.0/build-ps/percona-server-8.0_builder.sh -O percona-server_builder.sh
                                                                                                                                                sed -i 's:sudo::g' percona-server_builder.sh
                                                                                                                                                pwd -P
                                                                                                                                                ls -laR
                                                                                                                                                export build_dir=\$(pwd -P)
                                                                                                                                                docker run -u root -v \${build_dir}:\${build_dir} centos:6 sh -c "
                                                                                                                                                set -o xtrace
                                                                                                                                                cd \${build_dir}
                                                                                                                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --install_deps=1; \
                                                                                                                                                apt-get -y install git; \
                                                                                                                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --build_src_rpm=1"
                                                                                                                                            """
        stash(includes: 'srpm/*.rpm', name: 'srpm')
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
    string(defaultValue: 'master', description: 'Tag/Branch for PerconaFT repository', name: 'PERCONAFT_BRANCH')
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