pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        bat(script: 'gradle build', label: 'gradle build')
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        junit(testResults: 'build/reports/tests/test', allowEmptyResults: true)
      }
    }

    stage('mail') {
      steps {
        mail(subject: 'Build Success', body: 'New Build is deployed !', from: 'is_metidji@esi.dz', to: 'is_metidji@esi.dz')
      }
    }

  }
}