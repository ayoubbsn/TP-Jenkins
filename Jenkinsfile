pipeline {
  agent any
  stages {

    stage('test'){
        steps{
          bat 'gradlew test'
          archiveArtifacts 'build/test-results/'
          cucumber reportTitle: 'Report',
                   fileIncludePattern: 'target/report.json',
                   trendsLimit: 10
          junit 'build/test-results/test/TEST-Matrix.xml'
        }
    }

    stage ('Code Analysis') {
      steps{
        withSonarQubeEnv('sonar') {
          bat "gradlew sonarqube"
        }
      }
    }

    stage("Code Quality") {
      steps {
          waitForQualityGate abortPipeline: true
      }
    }

    stage('Build') {
      steps {
          bat "gradlew build"
          bat "gradlew javadoc"
          archiveArtifacts 'build/libs/*.jar'
          archiveArtifacts 'build/docs/'
      }
    }
    

    stage("Deploy") {
      steps {
          bat "gradlew publish"
      }
    }

    stage("Notification") {
      steps {
          notifyEvents message: 'Build Success', token: 'OjA6HA8u395GsIOMz0p4v5Rq-_8KOQ72'
      }
    }
}
  post {
        failure {
            mail bcc: '', body: '''Error !!''', cc: '', from: '', replyTo: '', subject: 'Pipleline failiure', to: 'ja_bousnane@esi.dz'
        }
  }

}
