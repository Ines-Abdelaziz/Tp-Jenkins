pipeline {
  agent any
  stages {
  stage ('Test') {
  steps {
  sh './gradlew test'
   junit 'build/test-results/test/TEST-Matrix.xml'

     cucumber buildStatus: 'UNSTABLE',
                  reportTitle: 'My report',
                  fileIncludePattern: 'target/report.json',

                  trendsLimit: 10

  }
}

}