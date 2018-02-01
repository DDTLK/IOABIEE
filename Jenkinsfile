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
    stage('Create project') {
      parallel {
        stage('Create project 1') {
          steps {
            unstash 'SDK_ID_1_NAME'
            sh '''

#unstash SDK_ID_1_NAME
SDK_ID_1_NAME=$(cat env_SDK_ID_1_NAME.txt)

#Create project and set id project
ID_1=$(xds-cli prj add --label="Project_hvac_"${SDK_ID_1_NAME}"" --type=pm --path=/home/jenkins/xds-workspace/hvac_"${SDK_ID_1_NAME}" --server-path=/home/devel/xds-workspace/hvac_"${SDK_ID_1_NAME}" | cut -d\')\' -f1 | cut -d\' \' -f5)
#save ID for futur stage
echo "${ID_1}" > env_ID_1.txt'''
            stash(name: 'ID_1', includes: 'env_ID_1.txt')
          }
        }
        stage('Create project 2') {
          steps {
            echo 'project...'
            unstash 'SDK_ID_2_NAME'
            sh '''
#unstash SDK_ID_2_NAME
SDK_ID_2_NAME=$(cat env_SDK_ID_2_NAME.txt)

#Create project and set id project
ID_2=$(xds-cli prj add --label="Project_hvac_"${SDK_ID_2_NAME}"" --type=pm --path=/home/jenkins/xds-workspace/hvac_"${SDK_ID_2_NAME}" --server-path=/home/devel/xds-workspace/hvac_"${SDK_ID_2_NAME}" | cut -d\')\' -f1 | cut -d\' \' -f5)

#save ID for futur stage
echo "${ID_2}" > env_ID_2.txt'''
            stash(name: 'ID_2', includes: 'env_ID_2.txt')
          }
        }
        stage('Create project 3') {
          steps {
            echo 'project...'
            unstash 'SDK_ID_3_NAME'
            sh '''
#unstash SDK_ID_3_NAME
SDK_ID_3_NAME=$(cat env_SDK_ID_3_NAME.txt)

#Create project and set id project
ID_3=$(xds-cli prj add --label="Project_hvac_"${SDK_ID_3_NAME}"" --type=pm --path=/home/jenkins/xds-workspace/hvac_"${SDK_ID_3_NAME}" --server-path=/home/devel/xds-workspace/hvac_"${SDK_ID_3_NAME}" | cut -d\')\' -f1 | cut -d\' \' -f5)

#save ID for futur stage
echo "${ID_3}" > env_ID_3.txt'''
            stash(name: 'ID_3', includes: 'env_ID_3.txt')
          }
        }
        stage('Create project 4') {
          steps {
            echo 'project...'
            unstash 'SDK_ID_4_NAME'
            sh '''
#unstash SDK_ID_4_NAME
SDK_ID_4_NAME=$(cat env_SDK_ID_4_NAME.txt)

#Create project and set id project
ID_4=$(xds-cli prj add --label="Project_hvac_"${SDK_ID_4_NAME}"" --type=pm --path=/home/jenkins/xds-workspace/hvac_"${SDK_ID_4_NAME}" --server-path=/home/devel/xds-workspace/hvac_"${SDK_ID_4_NAME}" | cut -d\')\' -f1 | cut -d\' \' -f5)

#save ID for futur stage
echo "${ID_4}" > env_ID_4.txt'''
            stash(name: 'ID_4', includes: 'env_ID_4.txt')
          }
        }
      }
    }
    stage('Build') {
      parallel {
        stage('Build SDK_ID_1') {
          steps {
            echo 'Build ....'
            unstash 'SDK_ID_1'
            unstash 'SDK_ID_1_NAME'
            unstash 'ID_1'
            lock(resource: 'server') {
              sh '''
#unstash variables
ID_1=$(cat env_ID_1.txt)
SDK_ID_1=$(cat env_SDK_ID_1.txt)
SDK_ID_1_NAME=$(cat env_SDK_ID_1_NAME.txt)

sleep 3
#Build
xds-cli exec --id="$ID_1" --sdkid="$SDK_ID_1" -- "qmake"

xds-cli exec --id="$ID_1" --sdkid="$SDK_ID_1" -- "make"

#Copy widget
cp ~/xds-workspace/hvac_"$SDK_ID_1_NAME"/package/hvac.wgt hvac_"$SDK_ID_1_NAME".wgt'''
            }
            
          }
        }
        stage('Build  SDK_ID_2') {
          steps {
            echo 'Build ...'
            unstash 'SDK_ID_2'
            unstash 'SDK_ID_2_NAME'
            unstash 'ID_2'
            lock(resource: 'server') {
              sh '''
#unstash variables
ID_2=$(cat env_ID_2.txt)
SDK_ID_2=$(cat env_SDK_ID_2.txt)
SDK_ID_2_NAME=$(cat env_SDK_ID_2_NAME.txt)
sleep 3

#Build
xds-cli exec --id="$ID_2" --sdkid="$SDK_ID_2" -- "qmake"

xds-cli exec --id="$ID_2" --sdkid="$SDK_ID_2" -- "make"

#Copy widget
cp ~/xds-workspace/hvac_"$SDK_ID_2_NAME"/package/hvac.wgt hvac_"$SDK_ID_2_NAME".wgt'''
            }
            
          }
        }
        stage('Build  SDK_ID_3') {
          steps {
            echo 'Build ...'
            unstash 'SDK_ID_3'
            unstash 'SDK_ID_3_NAME'
            unstash 'ID_3'
            lock(resource: 'server') {
              sh '''

#unstash variables
ID_3=$(cat env_ID_3.txt)
SDK_ID_3=$(cat env_SDK_ID_3.txt)
SDK_ID_3_NAME=$(cat env_SDK_ID_3_NAME.txt)
sleep 3

#Build
xds-cli exec --id="$ID_3" --sdkid="$SDK_ID_3" -- "qmake"

xds-cli exec --id="$ID_3" --sdkid="$SDK_ID_3" -- "make"

#Copy widget
cp ~/xds-workspace/hvac_"$SDK_ID_3_NAME"/package/hvac.wgt hvac_"$SDK_ID_3_NAME".wgt'''
            }
            
          }
        }
        stage('Build  SDK_ID_4') {
          steps {
            echo 'Build ...'
            unstash 'SDK_ID_4'
            unstash 'SDK_ID_4_NAME'
            unstash 'ID_4'
            lock(resource: 'server') {
              sh '''
#unstash variables
ID_4=$(cat env_ID_4.txt)
SDK_ID_4=$(cat env_SDK_ID_4.txt)
SDK_ID_4_NAME=$(cat env_SDK_ID_4_NAME.txt)
sleep 3

#Build
xds-cli exec --id="$ID_4" --sdkid="$SDK_ID_4" -- "qmake"

xds-cli exec --id="$ID_4" --sdkid="$SDK_ID_4" -- "make"

#Copy widget
cp ~/xds-workspace/hvac_"$SDK_ID_4_NAME"/package/hvac.wgt hvac_"$SDK_ID_4_NAME".wgt'''
            }
            
          }
        }
      }
    }
    stage('Publish') {
      steps {
        echo 'Publish'
        archiveArtifacts(artifacts: 'hvac_*.wgt', onlyIfSuccessful: true)
        deleteDir()
      }
    }
    stage('Clean') {
      steps {
        unstash 'ID_1'
        unstash 'ID_2'
        unstash 'ID_3'
        unstash 'ID_4'
        sh '''
#unstash ID
ID_1=$(cat env_ID_1.txt)
ID_2=$(cat env_ID_2.txt)
ID_3=$(cat env_ID_3.txt)
ID_4=$(cat env_ID_4.txt)

#remove project
echo "yes" | xds-cli prj rm --id="${ID_1}"
echo "yes" | xds-cli prj rm --id="${ID_2}"
echo "yes" | xds-cli prj rm --id="${ID_3}"
echo "yes" | xds-cli prj rm --id="${ID_4}"

#remove building directory
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