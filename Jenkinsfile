pipeline {
  agent any
  stages {
    
    stage('build') {
      post {
        success {
          mail(subject: 'Build Success', body: 'New Build is deployed !', from: 'is_metidji@esi.dz', to: 'im_aliousalah@esi.dz')
        }

        failure {
          mail(subject: 'Build Failure', body: 'New Build is deployed !', from: 'is_metidji@esi.dz', to: 'im_aliousalah@esi.dz')
        }

      }
      steps {
        bat(script: 'gradle build', label: 'gradle build')
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        junit(testResults: 'build/reports/tests/test', allowEmptyResults: true)
      }
    }
    
      stage('SonarQube analysis') {
       
           steps {
              withSonarQubeEnv() {
                 bat "sonar-scanner"   
              }
             script{
                def qualitygate = waitForQualityGate()
                if (qualitygate.status != "OK") {
                   error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
                }
             }

           } 

    }
    
    stage('Publish') {
      steps {
        bat 'gradle publish'
      }
    }

    stage('Slack Notifications') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', token: 'T02S71TC7EH/B02SZN07R08/df3SCOkFu6oGC9VYtrwEVUD6', message: 'New build is Created', channel: 'OGL')
      }
    }

  }
}
