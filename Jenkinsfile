pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        bat(script: 'gradle build', label: 'gradle build')
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        junit 'build/reports/tests/test'
      }
    }

  }
}