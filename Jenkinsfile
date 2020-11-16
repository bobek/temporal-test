echo currentBuild.getResult()
echo currentBuild.getPreviousBuild()?.getResult()
echo currentBuild.getBuildCauses().toString()
echo currentBuild.getPreviousCompletedBuild()?.getResult()

String cron_schedule = BRANCH_NAME == "develop" ? "* * * * *" : ""

// [{"_class":"hudson.model.Cause$UserIdCause","shortDescription":"Started by user mayastor","userId":"mayastor","userName":"mayastor"}]
// [{"_class":"jenkins.branch.BranchIndexingCause","shortDescription":"Branch indexing"}]
// [{"_class":"hudson.triggers.TimerTrigger$TimerTriggerCause","shortDescription":"Started by timer"}]


def getLastNonAbortedBuild(build) {
  if (build == null) {
    return null;
  }

  if(build.result.toString().equals("ABORTED")) {
    return getLastNonAbortedBuild(build.getPreviousBuild());
  } else {
    return build;
  }
}

echo getLastNonAbortedBuild(currentBuild.getPreviousBuild())?.toString()
echo getLastNonAbortedBuild(currentBuild.getPreviousBuild())?.getResult()

// if (currentBuild.rawBuild.getCauses().toString().contains('BranchIndexingCause')) {
if (currentBuild.getBuildCauses('hudson.model.Cause$BranchIndexingCause')) {
  print "INFO: Branch Indexing, aborting job"
//  currentBuild.result = 'ABORTED'
//  return
}

if (currentBuild.getBuildCauses('jenkins.branch.BranchIndexingCause') &&
    BRANCH_NAME == "develop") {
  print "INFO: Branch Indexing, aborting job"
  currentBuild.result = 'ABORTED'
  return
}

pipeline {
  agent any
  triggers {
    cron(cron_schedule)
  }


    stages {
        stage('Build') {
            when {
              beforeAngent true
              noneOf {
                branch 'develop'
                branch 'arne'
              }
            }
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
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
