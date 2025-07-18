pipeline {
  agent any

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
          echo ">>> Making gradlew executable"
          chmod +x android/gradlew

          echo ">>> Starting APK build"
          cd android
          ./gradlew clean
          ./gradlew assembleRelease --stacktrace
        '''
      }
    }

    stage('Verify APK') {
      steps {
        sh 'ls -l android/app/build/outputs/apk/release/'
      }
    }

    stage('Archive APK') {
      steps {
        archiveArtifacts artifacts: 'android/app/build/outputs/apk/release/app-release.apk', fingerprint: true
      }
    }
  }
}
