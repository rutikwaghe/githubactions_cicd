pipeline {
  agent any

  environment {
    NODE_HOME = tool name: 'NodeJS 18', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
    JAVA_HOME = '/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home'
    ANDROID_HOME = "${HOME}/Library/Android/sdk"
    PATH = "${NODE_HOME}/bin:$JAVA_HOME/bin:$ANDROID_HOME/platform-tools:$ANDROID_HOME/tools:$PATH"
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
        sh '''
          chmod +x android/gradlew
          ./android/gradlew assembleRelease -p android
        '''
      }
    }

    stage('Archive APK') {
      steps {
        archiveArtifacts artifacts: '**/app/build/outputs/**/*.apk', fingerprint: true
      }
    }
  }
}
