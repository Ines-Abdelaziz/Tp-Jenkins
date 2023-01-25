pipeline {
  agent any
  stages {
  stage ('Test') {
  steps {
  bat 'gradlew test'
   junit 'build/test-results/test/*.xml'

     cucumber buildStatus: 'UNSTABLE',
                  reportTitle: 'My report',
                  fileIncludePattern: 'target/report.json',

                  trendsLimit: 10

  }
}
stage('SonarQube analysis') {
      steps{
       withSonarQubeEnv("sonar") {
         bat 'gradlew sonarqube' }
      }

    }
stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
     stage("Build") {
            steps {
                bat 'gradlew build'
                bat 'gradlew javadoc'
                archiveArtifacts 'build/libs/*.jar'
                archiveArtifacts 'build/docs/'
            }
        }
     stage("deploy") {
            steps {
                bat 'gradlew publish'

            }
        }

    stage("notify"){
     post {
        always {
        echo "End of Pipeline process"
        mail(subject: 'End of Process Pipeline : Result incoming ...', body: 'End of Process Pipeline : Result incoming ...', from: 'ji_abdelaziz@esi.dz', to: 'ines.abdelaziz19@gmail.com')
      }
      failure {
        echo "Deployment failed"
        mail(subject: 'Deployment failed', body: 'Deployment failed ', from: 'ji_abdelaziz@esi.dz', to: 'ines.abdelaziz19@gmail.com')
      }
      success {
        echo "Deployment succeeded"
        mail(subject: 'Deployment succeeded', body: 'Deployment succeeded ', from: 'ji_abdelaziz@esi.dz', to: 'ines.abdelaziz19@gmail.com')
        notifyEvents message: 'Bonjour! : <b>Déploiement éffectué !</b> ! ', token: 'eBhj313rKyK1XRTk5VJuubTmjvJrJ7ej'
      }
    }
    }
}}