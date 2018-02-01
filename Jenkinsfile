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
        sh '''
#list sdks id install
SDKS_LIST=$( xds-cli sdks ls | cut -d\' \' -f1 )

#set the ID variables
SDK_ID_1=$( echo $SDKS_LIST | cut -d \' \' -f3)
SDK_ID_2=$( echo $SDKS_LIST | cut -d \' \' -f4)
SDK_ID_3=$( echo $SDKS_LIST | cut -d \' \' -f5)
SDK_ID_4=$( echo $SDKS_LIST | cut -d \' \' -f6)

#set the name for the ID corespondance
SDK_ID_1_NAME=$(xds-cli sdks ls | grep $SDK_ID_1 | cut -d\' \' -f5)
SDK_ID_2_NAME=$(xds-cli sdks ls | grep $SDK_ID_2 | cut -d\' \' -f5)
SDK_ID_3_NAME=$(xds-cli sdks ls | grep $SDK_ID_3 | cut -d\' \' -f5)
SDK_ID_4_NAME=$(xds-cli sdks ls | grep $SDK_ID_4 | cut -d\' \' -f5)


#create building repository
mkdir -p $HOME/xds-workspace/hvac_"${SDK_ID_1_NAME}"/
mkdir -p $HOME/xds-workspace/hvac_"${SDK_ID_2_NAME}"/
mkdir -p $HOME/xds-workspace/hvac_"${SDK_ID_3_NAME}"/
mkdir -p $HOME/xds-workspace/hvac_"${SDK_ID_4_NAME}"/

#copy sources into build directory
cp -r * $HOME/xds-workspace/hvac_"${SDK_ID_1_NAME}"/
cp -r * $HOME/xds-workspace/hvac_"${SDK_ID_2_NAME}"/
cp -r * $HOME/xds-workspace/hvac_"${SDK_ID_3_NAME}"/
cp -r * $HOME/xds-workspace/hvac_"${SDK_ID_4_NAME}"/

#Create project and set id project
ID_1=$(xds-cli prj add --label="Project_hvac_"${SDK_ID_1_NAME}"" --type=pm --path=/home/jenkins/xds-workspace/hvac_"${SDK_ID_1_NAME}" --server-path=/home/devel/xds-workspace/hvac_"${SDK_ID_1_NAME}" | cut -d\')\' -f1 | cut -d\' \' -f5)
ID_2=$(xds-cli prj add --label="Project_hvac_"${SDK_ID_2_NAME}"" --type=pm --path=/home/jenkins/xds-workspace/hvac_"${SDK_ID_1_NAME}" --server-path=/home/devel/xds-workspace/hvac_"${SDK_ID_2_NAME}" | cut -d\')\' -f1 | cut -d\' \' -f5)
ID_3=$(xds-cli prj add --label="Project_hvac_"${SDK_ID_3_NAME}"" --type=pm --path=/home/jenkins/xds-workspace/hvac_"${SDK_ID_1_NAME}" --server-path=/home/devel/xds-workspace/hvac_"${SDK_ID_3_NAME}" | cut -d\')\' -f1 | cut -d\' \' -f5)
ID_4=$(xds-cli prj add --label="Project_hvac_"${SDK_ID_4_NAME}"" --type=pm --path=/home/jenkins/xds-workspace/hvac_"${SDK_ID_1_NAME}" --server-path=/home/devel/xds-workspace/hvac_"${SDK_ID_4_NAME}" | cut -d\')\' -f1 | cut -d\' \' -f5)

#save ID for futur stage
echo "${ID_1}" > env_ID_1.txt
echo "${ID_2}" > env_ID_2.txt
echo "${ID_3}" > env_ID_3.txt
echo "${ID_4}" > env_ID_4.txt

#save SDK_ID for futur stage
echo "${SDK_ID_1_NAME}" > env_SDK_ID_1_NAME.txt
echo "${SDK_ID_2_NAME}" > env_SDK_ID_2_NAME.txt
echo "${SDK_ID_3_NAME}" > env_SDK_ID_3_NAME.txt
echo "${SDK_ID_4_NAME}" > env_SDK_ID_4_NAME.txt

#save SDK_ID_NAME for futur stage
echo "${SDK_ID_1}" > env_SDK_ID_1.txt
echo "${SDK_ID_2}" > env_SDK_ID_2.txt
echo "${SDK_ID_3}" > env_SDK_ID_3.txt
echo "${SDK_ID_4}" > env_SDK_ID_4.txt'''
        stash(name: 'ID_1', includes: 'env_ID_1.txt')
        stash(name: 'ID_2', includes: 'env_ID_2.txt')
        stash(name: 'ID_3', includes: 'env_ID_3.txt')
        stash(name: 'ID_4', includes: 'env_ID_4.txt')
        stash(includes: 'env_SDK_ID_1.txt', name: 'SDK_ID_1')
        stash(includes: 'env_SDK_ID_2.txt', name: 'SDK_ID_2')
        stash(includes: 'env_SDK_ID_3.txt', name: 'SDK_ID_3')
        stash(includes: 'env_SDK_ID_4.txt', name: 'SDK_ID_4')
        stash(includes: 'env_SDK_ID_1_NAME.txt', name: 'SDK_ID_1_NAME')
        stash(includes: 'env_SDK_ID_2_NAME.txt', name: 'SDK_ID_2_NAME')
        stash(includes: 'env_SDK_ID_3_NAME.txt', name: 'SDK_ID_3_NAME')
        stash(includes: 'env_SDK_ID_4_NAME.txt', name: 'SDK_ID_4_NAME')
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
        archiveArtifacts(artifacts: 'hvac_*.wgt', onlyIfSuccessful: true)
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