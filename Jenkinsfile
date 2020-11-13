echo currentBuild.getResult()
echo currentBuild.getPreviousBuild()?.getResult()

// if (currentBuild.rawBuild.getCauses().toString().contains('BranchIndexingCause')) {
if (currentBuild.getBuildCauses('hudson.model.Cause$BranchIndexingCause')) {
  print "INFO: Branch Indexing, aborting job"
  currentBuild.result = 'ABORTED'
  return
}

pipeline {
    agent any

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
}

echo currentBuild.getResult()
echo currentBuild.getPreviousBuild()?.getResult()
