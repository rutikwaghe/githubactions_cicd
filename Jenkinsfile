pipeline {
  agent any

  tools {
    nodejs 'NodeJS 24.4.1'
  }

  environment {
    ANDROID_HOME = "${env.HOME}/Library/Android/sdk"
    PATH = "${env.PATH}:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/cmdline-tools/latest/bin"
  }

  stages {

    stage('Checkout Code') {
      steps {
        git branch: 'main', url: 'https://github.com/rutikwaghe/githubactions_cicd.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        echo '📦 Installing node_modules...'
        sh 'npm install'
      }
    }

    stage('Clean & Build APK') {
      steps {
        echo '🏗️ Building release APK...'
        sh '''
          cd android
          chmod +x ./gradlew
          ./gradlew clean
          ./gradlew assembleRelease --no-daemon --stacktrace
        '''
      }
    }

    stage('List APK') {
      steps {
        echo '📁 Listing built APKs...'
        sh 'ls -lh android/app/build/outputs/apk/release/'
      }
    }

    stage('Archive APK') {
      steps {
        echo '📦 Archiving the APK...'
        archiveArtifacts artifacts: 'android/app/build/outputs/apk/release/app-release.apk', fingerprint: true
      }
    }
  }

  post {
    success {
      echo '✅ APK build successful!'
    }
    failure {
      echo '❌ Build failed. Please check the logs.'
    }
  }
}
