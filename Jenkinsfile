#!/usr/bin/env groovy

pipeline {
    agent any
    options {
        buildDiscarder (
            // only keep the latest builds
            logRotator(numToKeepStr:'10')
        )
    }
    stages {

        stage('Cleanup') {
            steps {
                sh 'git clean -nXdff'
            }
        }

        stage('[Docker] Build') {
            steps {
                sh 'docker build -t finch .'
            }
        }

        stage('[Docker] Test') {
            steps {
                sh '''
                docker run -v $(pwd)/tests:/code/tests finch /bin/bash -c " \
                pip install pytest flake8 && \
                pytest -v -m 'not slow and not online'"
                '''
            }
        }

        stage('Setup Miniconda') {
            steps {
                 sh '''
                 if ! [[ -d $HOME/miniconda ]] ; then
                    wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O $HOME/miniconda.sh
                    chmod +x $HOME/miniconda.sh
                    bash $HOME/miniconda.sh -b -p $HOME/miniconda
                 fi
                 '''
            }
        }

        stage('Install') {
            steps {
                sh '''#!/bin/bash -ex
                  export PATH="$HOME/miniconda/bin:$PATH"
                  make install
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''#!/bin/bash -ex
                  export PATH="$HOME/miniconda/bin:$PATH"
                  make test
                '''
            }
        }

        stage('[Docker] deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
