pipeline {
  agent any

  tools {
    nodejs 'NodeJS'
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
        echo 'Installing node_modules...'
        sh 'npm install'
      }
    }

    // -------------------------------
    // Android Build Stages
    // -------------------------------
    stage('Build Android APK') {
      steps {
        echo 'Building Android APK...'
        sh '''
          cd android
          chmod +x ./gradlew
          ./gradlew clean
          ./gradlew assembleRelease --no-daemon --stacktrace
        '''
      }
    }

    stage('Archive Android APK') {
      steps {
        echo 'Archiving Android APK...'
        archiveArtifacts artifacts: 'android/app/build/outputs/apk/release/app-release.apk', fingerprint: true
      }
    }

    // -------------------------------
    // iOS Build Stages
    // -------------------------------
    stage('Build iOS Enterprise IPA') {
      agent { label 'macos' } // Runs only on macOS node
      environment {
        BUILD_DIR = 'ios/build'
        ARCHIVE_PATH = "${BUILD_DIR}/otagithubactions.xcarchive"
        EXPORT_PATH = "${BUILD_DIR}/export"
        EXPORT_OPTIONS_PLIST = 'ios/ExportOptions.plist'
        SCHEME = 'otagithubactions'
        WORKSPACE = 'ios/otagithubactions.xcworkspace'
        CONFIGURATION = 'Release'
      }
      steps {
        echo 'Building iOS Enterprise IPA...'

        sh '''
          cd ios
          pod install
        '''

        sh """
          xcodebuild clean -workspace $WORKSPACE -scheme $SCHEME -configuration $CONFIGURATION

          xcodebuild archive \
            -workspace $WORKSPACE \
            -scheme $SCHEME \
            -configuration $CONFIGURATION \
            -archivePath $ARCHIVE_PATH \
            -allowProvisioningUpdates \
            -destination 'generic/platform=iOS'
        """

        sh """
          xcodebuild -exportArchive \
            -archivePath $ARCHIVE_PATH \
            -exportOptionsPlist $EXPORT_OPTIONS_PLIST \
            -exportPath $EXPORT_PATH
        """
      }
    }

    stage('Archive iOS IPA') {
      agent { label 'macos' }
      steps {
        echo 'Archiving iOS IPA...'
        archiveArtifacts artifacts: 'ios/build/export/*.ipa', fingerprint: true
      }
    }
  }

  post {
    success {
      echo 'Build completed successfully!'
    }
    failure {
      echo 'Build failed. Please check logs.'
    }
  }
}



// pipeline {
//   agent any

//   tools {
//     nodejs 'NodeJS'
//   }

//   environment {
//     ANDROID_HOME = "${env.HOME}/Library/Android/sdk"
//     PATH = "${env.PATH}:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/cmdline-tools/latest/bin"
//   }

//   stages {

//     stage('Checkout Code') {
//       steps {
//         git branch: 'main', url: 'https://github.com/rutikwaghe/githubactions_cicd.git'
//       }
//     }

//     stage('Install Dependencies') {
//       steps {
//         echo 'Installing node_modules...'
//         sh 'npm install'
//       }
//     }

//     stage('Clean & Build APK') {
//       steps {
//         echo '🏗️ Building release APK...'
//         sh '''
//           cd android
//           chmod +x ./gradlew
//           ./gradlew clean
//           ./gradlew assembleRelease --no-daemon --stacktrace
//         '''
//       }
//     }

//     stage('List APK') {
//       steps {
//         echo 'Listing built APKs...'
//         sh 'ls -lh android/app/build/outputs/apk/release/'
//       }
//     }

//     stage('Archive APK') {
//       steps {
//         echo 'Archiving the APK...'
//         archiveArtifacts artifacts: 'android/app/build/outputs/apk/release/app-release.apk', fingerprint: true
//       }
//     }
//   }

//   post {
//     success {
//       echo 'APK build successful!'
//     }
//     failure {
//       echo 'Build failed. Please check the logs.'
//     }
//   }
// }
