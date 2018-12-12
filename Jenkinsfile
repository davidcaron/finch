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
        stage('Docker build') {
            steps {
                sh 'docker build -t finch .'
            }
        }

        stage('Docker test') {
            steps {
                sh '''
                docker run finch \
                conda install -y -c conda-forge pytest flake8 && \
                pytest -v -m 'not slow and not online'
                '''
                docker run -v $(pwd)/tests:/code/tests finch /bin/bash -c " \
                pip install pytest flake8 && \
                pytest -v -m 'not slow and not online'"
            }
            
        }
        stage('Docker deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
