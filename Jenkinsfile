pipeline {
  agent any
  tools{
	nodejs 'NodeJS 12.4.0'
  }
  stages {
    stage('Compile') {
      agent any
      steps {
        sh 'npm install'
        sh 'npm run-script build'
	stash includes : 'public/**/*' , name:'stashfiles'
      }
    }
    stage('Unit Testing') {
      agent any
      steps {
        sh 'npm test'
      }
    }
    stage('Deploy') {
      parallel {
        stage('Deploy') {
          steps {
	    unstash 'stashfiles'	  
            sshPublisher(publishers: [sshPublisherDesc(configName: 'student1', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'public', sourceFiles: 'public/**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	  }
        }
        stage('Archive Artifacts') {
          steps {
            archiveArtifacts '**/public/assets/**'
          }
        }
      }
    }
  }
}
