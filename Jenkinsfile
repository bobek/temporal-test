echo currentBuild.getResult()
echo currentBuild.getPreviousBuild()?.getResult()
echo currentBuild.getBuildCauses().toString()

// if (currentBuild.rawBuild.getCauses().toString().contains('BranchIndexingCause')) {
if (currentBuild.getBuildCauses('hudson.model.Cause$BranchIndexingCause')) {
  print "INFO: Branch Indexing, aborting job"
//  currentBuild.result = 'ABORTED'
//  return
}

pipeline {
  agent any
  triggers {
    cron('* * * * *')
  }


    stages {
        stage('Build') {
        when {
          beforeAgent true
          anyOf {
            allOf {
              branch 'staging'
              not { triggeredBy 'TimerTrigger' }
            }
            allOf {
              branch 'trying'
              not { triggeredBy 'TimerTrigger' }
            }
            allOf {
              branch 'develop'
              anyOf {
                triggeredBy 'TimerTrigger'
                triggeredBy cause: 'UserIdCause'
              }
            }
          }
        }

            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
        when {
          beforeAgent true
          anyOf {
            allOf {
              branch 'staging'
              not { triggeredBy 'TimerTrigger' }
            }
            allOf {
              branch 'trying'
              not { triggeredBy 'TimerTrigger' }
            }
            allOf {
              branch 'develop'
              anyOf {
                triggeredBy 'TimerTrigger'
                triggeredBy cause: 'UserIdCause'
              }
            }
          }
        }
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
        when {
          beforeAgent true
          anyOf {
            allOf {
              branch 'staging'
              not { triggeredBy 'TimerTrigger' }
            }
            allOf {
              branch 'trying'
              not { triggeredBy 'TimerTrigger' }
            }
            allOf {
              branch 'develop'
              anyOf {
                triggeredBy 'TimerTrigger'
                triggeredBy cause: 'UserIdCause'
              }
            }
          }
        }
            steps {
                echo 'Deploying....'
            }
        }
    }

    post {
      always {
        node(null) {
          script {
            echo "POST"
          }
        }
      }
    }
}

echo currentBuild.getResult()
echo currentBuild.getPreviousBuild()?.getResult()
