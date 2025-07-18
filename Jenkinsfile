pipeline {
  agent any

  environment {
    NODE_HOME = tool name: 'NodeJS 18', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
    PATH = "${NODE_HOME}/bin:$PATH"
  }

  tools {
    nodejs 'NodeJS 18'
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/rutikwaghe/githubactions_cicd.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }

    stage('Build Android') {
      steps {
        sh './android/gradlew assembleRelease -p android'
      }
    }

    stage('Archive APK') {
      steps {
        archiveArtifacts artifacts: '**/app/build/outputs/**/*.apk', fingerprint: true
      }
    }
  }
}
