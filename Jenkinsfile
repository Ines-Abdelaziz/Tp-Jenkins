pipeline {
  agent any
  stages {
  stage ('Test') {
  steps {
  bat 'gradle test'
   junit 'build/test-results/test/*.xml'

     cucumber buildStatus: 'UNSTABLE',
                  reportTitle: 'My report',
                  fileIncludePattern: 'target/report.json',

                  trendsLimit: 10

  }
}

}}