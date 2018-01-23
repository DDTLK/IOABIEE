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
        dir(path: '~/xds_workspace') {
          git 'https://gerrit.automotivelinux.org/gerrit/apps/hvac'
          dir(path: 'hvac') {
            echo 'set ID & SDK_ID'
            sh '''
export SDK_ID=$( xds-cli sdks ls | cut -d\' \' -f1 | tail -n1 )

export ID=$(xds-cli prj add --label="Project_hvac" --type=pm --path=/home/jenkins/xds-workspace/hvac --server-path=/home/devel/xds-workspace/hvac | cut -d\')\' -f1 | cut -d\' \' -f5)


xds-cli exec --id="$ID" --sdkid="$SDK_ID" -- "qmake"

xds-cli exec --id="$ID" --sdkid="$SDK_ID" -- "make"

cp ~/xds-workspace/hvac/package/hvac.wgt .

 echo "${ID}"'''
          }
          
        }
        
      }
    }
    stage('Publish') {
      steps {
        echo 'Publish'
        archiveArtifacts(artifacts: '~/xds_workspace/hvac/hvac.wgt', onlyIfSuccessful: true)
        sh ' echo "${ID}"'
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