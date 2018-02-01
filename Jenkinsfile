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
        sh 'PATH=$(pwd)'
        git 'https://gerrit.automotivelinux.org/gerrit/apps/hvac'
        dir(path: 'hvac') {
          echo 'set ID & SDK_ID'
          sh '''
export SDK_ID=$( xds-cli sdks ls | cut -d\' \' -f1 | tail -n1 )

export ID=$(xds-cli prj add --label="Project_hvac" --type=pm --path="$PATH" --server-path=/home/devel/xds-workspace/hvac | cut -d\')\' -f1 | cut -d\' \' -f5)


echo "${ID}" > env_ID.txt

echo "${SDK_ID}" > env_SDK_ID.txt'''
          stash(name: 'ID', includes: 'env_ID.txt')
          stash(includes: 'env_SDK_ID.txt', name: 'SDK_ID')
        }
        
      }
    }
    stage('Build') {
      steps {
        echo 'Build ....'
        unstash 'SDK_ID'
        unstash 'ID'
        sh '''ID=$(cat env_ID.txt)
SDK_ID=$(cat env_SDK_ID.txt)

xds-cli exec --id="$ID" --sdkid="$SDK_ID" -- "qmake"

xds-cli exec --id="$ID" --sdkid="$SDK_ID" -- "make"

cp ~/xds-workspace/hvac/package/hvac.wgt .'''
      }
    }
    stage('Publish') {
      steps {
        echo 'Publish'
        archiveArtifacts(artifacts: '~/xds_workspace/hvac/hvac.wgt', onlyIfSuccessful: true)
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