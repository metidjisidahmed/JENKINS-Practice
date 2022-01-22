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

    stage('mail') {
      steps {
        mail(subject: 'blabla', body: 'babla body', to: 'ia_bacha@esi.dz', from: 'is_metidji@esi.dz')
      }
    }

  }
}