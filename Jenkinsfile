pipeline {
  agent {
    node {
      label 'xds-tools'
    }
    
  }
  stages {
    stage('Setup') {
      steps {
        echo 'Setup project'
        git 'https://gerrit.automotivelinux.org/gerrit/apps/hvac'
        dir(path: 'hvac') {
          echo 'set ID & SDK_ID'
          sh '''SDK_ID=$( xds-cli sdks ls | cut -d\' \' -f1 | tail -n1 )
ID=$(xds-cli prj add --label="Project_hvac" --type=pm --path=/home/jenkins/xds-workspace/hvac --server-path=/home/devel/xds-workspace/hvac | cut -d\')\' -f1 | cut -d\' \' -f5)
'''
          echo 'prj_id="$ID"'
          echo 'sdk_id="$SDK_ID"'
          stash(name: 'ID', includes: 'ID')
          stash(name: 'SDK_ID', includes: 'SDK_ID')
        }
        
      }
    }
    stage('Build') {
      steps {
        echo 'Build ...'
        sh '''pwd
xds-cli exec --id="$ID" --sdkid="$SDK_ID" -- "qmake"
xds-cli exec --id="$ID" --sdkid="$SDK_ID" -- "make"'''
      }
    }
    stage('Publish') {
      steps {
        sh '''pwd
ls -l package/hvac.wgt '''
        archiveArtifacts 'hvac.wgt'
      }
    }
  }
  environment {
    PATH = '$PATH:/opt/AGL/xds/cli/:/opt/AGL/xds/agent/:/opt/AGL/xds/agent/gdb/:/usr/bin/:/usr/local/bin'
  }
  post {
    always {
      deleteDir()
      
    }
    
  }
}