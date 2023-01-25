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
stage('SonarQube analysis') {
      steps{
       withSonarQubeEnv("sonar") { // Will pick the global server connection you have configured
         bat 'gradle sonar' }
      }

    }
/*stage("Code Quality") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                   //
                    waitForQualityGate abortPipeline: true
                }
            }
        }*/
 stage("Build") {
            steps {
                bat 'gradle build'
                bat 'gradle javadoc'
                archiveArtifacts 'build/libs/*.jar'
                archiveArtifacts 'build/docs/'
            }
        }
  stage("deploy") {
             steps {
                 bat 'gradle publish'

             }
         }

}}