pipeline {
  agent {
    label 'docker'
  }
  stages {
    stage('Build percona-server source') {
      agent {
        node {
          label 'min-xenial-x86_64'
        }

      }
      steps {
        sh '''
            set -o xtrace
            sudo rm -rf test source_tarball tarball srpm sdeb rpms debs
            mkdir test
            wget https://raw.githubusercontent.com/EvgeniyPatlan/percona-server/8.0/build-ps/percona-server-8.0_builder.sh -O percona-server_builder.sh
            sed -i \'s:sudo::g\' percona-server_builder.sh
            pwd -P
            ls -laR
            export build_dir=$(pwd -P)
            docker run -u root -v ${build_dir}:${build_dir} ubuntu:xenial sh -c "
                set -o xtrace
                cd ${build_dir}
                apt-get -y update;
                bash -x ./percona-server_builder.sh --builddir=${build_dir}/test --install_deps=1;                                                                                                                                                                                                                                                                 apt-get -y install git;                                                                                                                                                                                                                                                                 bash -x ./percona-server_builder.sh --builddir=${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --get_sources=1
            "
        '''
        stash(includes: 'source_tarball/*', name: 'source_tarball')
      }
    }

    stage('Build generic source packages') {
      parallel {
        stage('Build PS generic source rpm') {
          steps {
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
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --build_src_rpm=1
                                            "
                                        """
            stash(includes: 'srpm/*', name: 'srpm')
          }
        }

        stage('Build source deb') {
          steps {
            sh """
                                            set -o xtrace
                                            rm -rf test
                                            mkdir test
                                            wget https://raw.githubusercontent.com/EvgeniyPatlan/percona-server/8.0/build-ps/percona-server-8.0_builder.sh -O percona-server_builder.sh
                                            sed -i 's:sudo::g' percona-server_builder.sh
                                            pwd -P
                                            ls -laR
                                            export build_dir=\$(pwd -P)
                                            docker run -u root -v \${build_dir}:\${build_dir} ubuntu:xenial sh -c "
                                                set -o xtrace
                                                cd \${build_dir}
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --install_deps=1; \
                                                apt-get -y install git; \
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --build_source_deb=1
                                            "
                                        """
            stash(includes: 'sdeb/*', name: 'sdeb')
          }
        }

      }
    }

    stage('Build packages') {
      parallel {
        stage('Centos7') {
          steps {
            sh """
                                            set -o xtrace
                                            rm -rf test
                                            mkdir test
                                            wget https://raw.githubusercontent.com/EvgeniyPatlan/percona-server/8.0/build-ps/percona-server-8.0_builder.sh -O percona-server_builder.sh
                                            sed -i 's:sudo::g' percona-server_builder.sh
                                            pwd -P
                                            ls -laR
                                            export build_dir=\$(pwd -P)
                                            docker run -u root -v \${build_dir}:\${build_dir} centos:7 sh -c "
                                                set -o xtrace
                                                cd \${build_dir}
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --install_deps=1; \
                                                apt-get -y install git; \
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --build_rpm=1
                                            "
                                        """
            stash(includes: 'rpm/*', name: 'rpm_cent7')
          }
        }

        stage('Centos 8') {
          steps {
            sh """
                                            set -o xtrace
                                            rm -rf test
                                            mkdir test
                                            wget https://raw.githubusercontent.com/EvgeniyPatlan/percona-server/8.0/build-ps/percona-server-8.0_builder.sh -O percona-server_builder.sh
                                            sed -i 's:sudo::g' percona-server_builder.sh
                                            pwd -P
                                            ls -laR
                                            export build_dir=\$(pwd -P)
                                            docker run -u root -v \${build_dir}:\${build_dir} centos:8 sh -c "
                                                set -o xtrace
                                                cd \${build_dir}
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --install_deps=1; \
                                                apt-get -y install git; \
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --build_rpm=1
                                            "
                                        """
            stash(includes: 'rpm/*', name: 'rpm_cent8')
          }
        }

        stage('Ubuntu 16.04') {
          steps {
            sh """
                                            set -o xtrace
                                            rm -rf test
                                            mkdir test
                                            wget https://raw.githubusercontent.com/EvgeniyPatlan/percona-server/8.0/build-ps/percona-server-8.0_builder.sh -O percona-server_builder.sh
                                            sed -i 's:sudo::g' percona-server_builder.sh
                                            pwd -P
                                            ls -laR
                                            export build_dir=\$(pwd -P)
                                            docker run -u root -v \${build_dir}:\${build_dir} ubuntu:xenial sh -c "
                                                set -o xtrace
                                                cd \${build_dir}
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --install_deps=1; \
                                                apt-get -y install git; \
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --build_deb=1
                                            "
                                        """
            stash(includes: 'deb/*', name: 'deb16')
          }
        }

        stage('Ubuntu 18.04') {
          steps {
            sh """
                                            set -o xtrace
                                            rm -rf test
                                            mkdir test
                                            wget https://raw.githubusercontent.com/EvgeniyPatlan/percona-server/8.0/build-ps/percona-server-8.0_builder.sh -O percona-server_builder.sh
                                            sed -i 's:sudo::g' percona-server_builder.sh
                                            pwd -P
                                            ls -laR
                                            export build_dir=\$(pwd -P)
                                            docker run -u root -v \${build_dir}:\${build_dir} ubuntu:bionic sh -c "
                                                set -o xtrace
                                                cd \${build_dir}
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --install_deps=1; \
                                                apt-get -y install git; \
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --build_deb=1
                                            "
                                        """
            stash(includes: 'deb/*', name: 'deb18')
          }
        }

        stage('Ubuntu 20.04') {
          steps {
            sh """
                                            set -o xtrace
                                            rm -rf test
                                            mkdir test
                                            wget https://raw.githubusercontent.com/EvgeniyPatlan/percona-server/8.0/build-ps/percona-server-8.0_builder.sh -O percona-server_builder.sh
                                            sed -i 's:sudo::g' percona-server_builder.sh
                                            pwd -P
                                            ls -laR
                                            export build_dir=\$(pwd -P)
                                            docker run -u root -v \${build_dir}:\${build_dir} ubuntu:focal sh -c "
                                                set -o xtrace
                                                cd \${build_dir}
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --install_deps=1; \
                                                apt-get -y install git; \
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --build_deb=1
                                            "
                                        """
            stash(includes: 'deb/*', name: 'deb20')
          }
        }

        stage('Debian 9') {
          steps {
            sh """
                                            set -o xtrace
                                            rm -rf test
                                            mkdir test
                                            wget https://raw.githubusercontent.com/EvgeniyPatlan/percona-server/8.0/build-ps/percona-server-8.0_builder.sh -O percona-server_builder.sh
                                            sed -i 's:sudo::g' percona-server_builder.sh
                                            pwd -P
                                            ls -laR
                                            export build_dir=\$(pwd -P)
                                            docker run -u root -v \${build_dir}:\${build_dir} debian:stretch sh -c "
                                                set -o xtrace
                                                cd \${build_dir}
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --install_deps=1; \
                                                apt-get -y install git; \
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --build_deb=1
                                            "
                                        """
            stash(includes: 'deb/*', name: 'deb9')
          }
        }

        stage('Debian 10') {
          steps {
            sh """
                                            set -o xtrace
                                            rm -rf test
                                            mkdir test
                                            wget https://raw.githubusercontent.com/EvgeniyPatlan/percona-server/8.0/build-ps/percona-server-8.0_builder.sh -O percona-server_builder.sh
                                            sed -i 's:sudo::g' percona-server_builder.sh
                                            pwd -P
                                            ls -laR
                                            export build_dir=\$(pwd -P)
                                            docker run -u root -v \${build_dir}:\${build_dir} debian:buster sh -c "
                                                set -o xtrace
                                                cd \${build_dir}
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --install_deps=1; \
                                                apt-get -y install git; \
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --build_deb=1
                                            "
                                        """
            stash(includes: 'deb/*', name: 'deb10')
          }
        }

        stage('Tarball') {
          steps {
            sh """
                                            set -o xtrace
                                            rm -rf test
                                            mkdir test
                                            wget https://raw.githubusercontent.com/EvgeniyPatlan/percona-server/8.0/build-ps/percona-server-8.0_builder.sh -O percona-server_builder.sh
                                            sed -i 's:sudo::g' percona-server_builder.sh
                                            pwd -P
                                            ls -laR
                                            export build_dir=\$(pwd -P)
                                            docker run -u root -v \${build_dir}:\${build_dir} ubuntu:xenial sh -c "
                                                set -o xtrace
                                                cd \${build_dir}
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --install_deps=1; \
                                                apt-get -y install git; \
                                                bash -x ./percona-server_builder.sh --builddir=\${build_dir}/test --repo=${GIT_REPO} --branch=${BRANCH} --rpm_release=${RPM_RELEASE} --deb_release=${DEB_RELEASE} --build_tarball=1
                                            "
                                        """
            stash(includes: 'tarball/*', name: 'tarball')
          }
        }

      }
    }

  }
  options {
    skipDefaultCheckout()
    disableConcurrentBuilds()
  }
  parameters {
    string(defaultValue: 'https://github.com/EvgeniyPatlan/percona-server.git', description: 'github repository for build', name: 'GIT_REPO')
    string(defaultValue: 'Percona-Server-8.0.20-11', description: 'Tag/Branch for percona-server repository', name: 'BRANCH')
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