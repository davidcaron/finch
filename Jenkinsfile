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
        stage('Build') {
            steps {
                sh 'docker build -t finch .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing..'
            }
            
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
