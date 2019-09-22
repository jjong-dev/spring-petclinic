pipeline {
  agent none
  stages {
    stage('build & test') {
      agent {
        node {
          label 'docker'
        }

      }
      steps {
        sh 'mvn -Dmaven.test.failure.ignore clean package'
        stash(name: 'build-test-artifacts', includes: '**/target/surefire-reports/TEST-*.xml,target/*.jar')
      }
    }
    stage('report & publish') {
      agent {
        node {
          label 'docker'
        }

      }
      steps {
        unstash 'build-test-artifacts'
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts(artifacts: 'target/*.jar', onlyIfSuccessful: true)
      }
    }
  }
}