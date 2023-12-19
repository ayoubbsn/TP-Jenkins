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

      stage('Build') {
          steps {
              bat "gradlew build"
              bat "gradlew javadoc"
              archiveArtifacts 'build/libs/*.jar'
              archiveArtifacts 'build/docs/'
          }
        }

      /*

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

    


    

    stage("Deploy") {
      steps {
          bat "gradlew publish"
      }
    }
    }*/


    stage("Notification") {
      steps {
          notifyEvents message: 'Build Success', token: 'xetr1vcbig3yobfxhs5g_zzvtw60bnge'
      }
    }
}


}
