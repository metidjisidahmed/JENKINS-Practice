pipeline {
  agent any
  stages {
    stage('build') {
      post {
        success {
          mail(subject: 'Build Success', body: 'New Build is deployed !', from: 'is_metidji@esi.dz', to: 'is_metidji@esi.dz')
        }
        failure {
          mail(subject: 'Build Failure', body: "the new build isn't deployed succesfully !", from: 'is_metidji@esi.dz', to: 'is_metidji@esi.dz')
        }
        
      }
      steps {
        bat(script: 'gradle build', label: 'gradle build')
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        junit(testResults: 'build/reports/tests/test', allowEmptyResults: true)
      }
    }
    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('TP8_OGL_JENKINS') {
              bat 'sonar-scanner'
            }


          }
        }

        stage('Cucumber report') {
          steps {
            cucumber 'target/report.json'
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
