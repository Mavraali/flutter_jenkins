pipeline {
    agent any

    environment {
        FLUTTER_HOME = "/usr/local/flutter"
        ANDROID_HOME = "/usr/local/android-sdk"
        PATH = "${env.PATH}:${env.FLUTTER_HOME}/bin:${env.ANDROID_HOME}/platform-tools"
    }

    stages {
        stage('Prepare Environment') {
            steps {
                script {
                    sh "echo 'PATH is $PATH'"
                    sh "echo 'Flutter path is $FLUTTER_HOME'"
                    sh "${FLUTTER_HOME}/bin/flutter --version"
                    sh "echo 'Android SDK path is $ANDROID_HOME'"
                    sh "ls -la ${ANDROID_HOME}"
                    sh "${FLUTTER_HOME}/bin/flutter doctor"
                }
            }
        }

        stage('Build Android') {
            steps {
                script {
                    sh '''
                    echo 'Exporting ANDROID_HOME'
                    export ANDROID_HOME=/usr/local/android-sdk
                    echo 'Checking Android SDK location'
                    ls -la $ANDROID_HOME
                    echo 'Cleaning project...'
                    flutter clean
                    echo 'Getting dependencies...'
                    flutter pub get
                    echo 'Building APK...'
                    flutter build apk --debug -v
                    '''
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh '''
                    echo 'Running tests...'
                    flutter test
                    '''
                }
            }
        }

        stage('Package Android') {
            steps {
                script {
                    sh '''
                    echo 'Building release APK...'
                    flutter build apk --release -v
                    '''
                }
            }
        }

        stage('Build iOS') {
            steps {
                script {
                    sh '''
                    echo 'Building iOS...'
                    flutter build ios --debug --no-codesign -v
                    '''
                }
            }
        }

        stage('Package iOS') {
            steps {
                script {
                    sh '''
                    echo 'Packaging iOS app...'
                    xcodebuild -workspace ios/Runner.xcworkspace -scheme Runner -configuration Debug -destination generic/platform=iOS build
                    '''
                }
            }
        }
    }

    post {
        always {
            // Clean up after the pipeline runs
            sh 'flutter clean'
        }
    }
}
//testing
