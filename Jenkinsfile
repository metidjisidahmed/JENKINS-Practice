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

        stage('Code Analysis') {
          parallel {
          stage('SonarQube analysis') {
        withSonarQubeEnv('My SonarQube Server') {
          sh 'mvn clean package sonar:sonar'
        } // submitted SonarQube taskId is automatically attached to the pipeline context
      }
    }

    // No need to occupy a node
    stage("Quality Gate"){
      timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
        def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
        if (qg.status != 'OK') {
          error "Pipeline aborted due to quality gate failure: ${qg.status}"
        }
      }

        stage('Test Reporting') {
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
