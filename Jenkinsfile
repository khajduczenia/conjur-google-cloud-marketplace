#!/usr/bin/env groovy

pipeline {
  agent { label 'executor-v2' }

  options {
    timestamps()
    buildDiscarder(logRotator(numToKeepStr: '30'))
  }

  stages {
    stage('Stubbed until GCL goes live') {
      steps {
        sh 'exit 0'
      }
    }
    // stage('Fetch marketplace submodule') {
    //   steps {
    //     sh 'git submodule sync --recursive'
    //     sh 'git submodule update --recursive --init --force'
    //   }
    // }
    // stage('Verify application') {
    //   steps {
    //     sh './test.sh'
    //   }
    // }
  }

  post {
    always {
      cleanupAndNotify(currentBuild.currentResult)
    }
  }
}
