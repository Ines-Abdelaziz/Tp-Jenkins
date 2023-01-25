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
  stage('Notify') {
          steps {
              echo "Notification..."
              notifyEvents message: 'Build is created with success', token: 'eBhj313rKyK1XRTk5VJuubTmjvJrJ7ej'
          }
      }
    }


    post {
      success {
          mail to: "ji_abdelaziz@esi.dz",
          subject: "Build Succeeded",
          body: "This is an email that informs that the new Build is deployed with success!"
      }
      failure {
          mail to: "jl_ji_abdelaziz@esi.dz",
          subject: "Build failed",
          body: "This is an email that informs that the new Build is deployed with failure!"
      }

}}