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
