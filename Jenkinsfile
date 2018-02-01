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
        sh '''export SDK_ID_1=$( xds-cli sdks ls | cut -d\' \' -f1 | tail -n1 )
SDK_ID_1_NAME=$(xds-cli sdks ls | grep $SDK_ID_1 | cut -d\' \' -f5)
mkdir -p $HOME/xds-workspace/hvac_"${SDK_ID_1_NAME}"/
cp -r * $HOME/xds-workspace/hvac_"${SDK_ID_1_NAME}"/'''
        echo 'set ID & SDK_ID'
        sh '''

export ID_1=$(xds-cli prj add --label="Project_hvac_"${SDK_ID_1_NAME}"" --type=pm --path=/home/jenkins/xds-workspace/hvac_"${SDK_ID_1_NAME}" --server-path=/home/devel/xds-workspace/hvac_"${SDK_ID_1_NAME}" | cut -d\')\' -f1 | cut -d\' \' -f5)


echo "${ID_1}" > env_ID_1.txt
echo "${SDK_ID_1_NAME}" > env_SDK_ID_1_NAME.txt
echo "${SDK_ID_1}" > env_SDK_ID_1.txt'''
        stash(name: 'ID_1', includes: 'env_ID_1.txt')
        stash(includes: 'env_SDK_ID_1.txt', name: 'SDK_ID_1')
        stash(includes: 'env_SDK_ID_1_NAME.txt', name: 'SDK_ID_1_NAME')
      }
    }
    stage('Build') {
      steps {
        echo 'Build ....'
        unstash 'SDK_ID_1'
        unstash 'SDK_ID_1_NAME'
        unstash 'ID_1'
        sh '''ID_1=$(cat env_ID_1.txt)
SDK_ID_1=$(cat env_SDK_ID_1.txt)
SDK_ID_1_NAME=$(cat env_SDK_ID_1_NAME.txt)


xds-cli exec --id="$ID_1" --sdkid="$SDK_ID_1" -- "qmake"

xds-cli exec --id="$ID_1" --sdkid="$SDK_ID_1" -- "make"

cp ~/xds-workspace/hvac_"$SDK_ID_1_NAME"/package/hvac.wgt hvac_"$SDK_ID_1_NAME".wgt'''
      }
    }
    stage('Publish') {
      steps {
        echo 'Publish'
        unstash 'SDK_ID_1_NAME'
        sh 'SDK_ID_1_NAME=$(cat env_SDK_ID_1_NAME.txt)'
        archiveArtifacts(artifacts: 'hvac_"$SDK_ID_1".wgt', onlyIfSuccessful: true)
        deleteDir()
      }
    }
    stage('Clean') {
      steps {
        unstash 'ID_1'
        sh '''ID_1=$(cat env_ID_1.txt)


echo "yes" | xds-cli prj rm --id="${ID_1}"
rm -rf $HOME/xds-workspace/hvac*'''
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