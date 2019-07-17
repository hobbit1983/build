def mvnProfile    = 'unknown'
def dockerVersion = 'unknown'
def gitBranch     = 'unknown'

pipeline {
// Initially run on any agent
   agent any
   environment {
//Configure Maven from the maven tooling in Jenkins
      def mvnHome = tool 'Default'
      PATH = "${mvnHome}/bin:${env.PATH}"
      
//Set some defaults
      def workspace = pwd()
      def mvnGoal    = 'deploy'
      def dockerRepository = 'cicsts-docker-local.artifactory.swg-devops.com'
   }
   stages {
// If it is the master branch, version 0.3.0 and master on all the other branches
      stage('set-dev') {
         when {
           environment name: 'GIT_BRANCH', value: 'origin/master'
         }
         steps {
            script {
               mvnProfile    = 'voras-dev'
               dockerVersion = '0.3.0'
               gitBranch     = 'master'
            }
         }
      }
// If the test-preprod tag,  then set as appropriate
      stage('set-test-preprod') {
         when {
           environment name: 'GIT_BRANCH', value: 'origin/testpreprod'
         }
         steps {
            script {
               mvnProfile    = 'voras-preprod'
               dockerVersion = 'preprod'
               gitBranch     = 'testpreprod'
            }
         }
      }

// for debugging purposes
      stage('report') {
         steps {
            echo "Branch/Tag         : ${env.GIT_BRANCH}"
            echo "Repo Branches      : ${gitBranch}"
            echo "Workspace directory: ${workspace}"
            echo "Maven Goal         : ${mvnGoal}"
            echo "Maven profile      : ${mvnProfile}"
            echo "Docker Version     : ${dockerVersion}"
            echo "Docker Repository  : ${dockerRepository}"
         }
      }
   
// Set up the workspace, clear the git directories and setup the manve settings.xml files
      stage('prep-workspace') { 
         steps {
            configFileProvider([configFile(fileId: '86dde059-684b-4300-b595-64e83c2dd217', targetLocation: 'settings.xml')]) {
            }
            configFileProvider([configFile(fileId: '647688a9-d178-475b-ae65-80fca764a8c2', targetLocation: 'cit-settings.xml')]) {
            }
         
            dir('repository/dev/voras') {
               deleteDir()
            }
            dir('git/wrapping') {
               deleteDir()
            }
            dir('git/maven') {
               deleteDir()
            }
            dir('git/framework') {
               deleteDir()
            }
            dir('git/core') {
               deleteDir()
            }
            dir('git/common') {
               deleteDir()
            }
            dir('git/runtime') {
               deleteDir()
            }
            dir('git/devtools') {
               deleteDir()
            }
            dir('git/ivt') {
               deleteDir()
            }
            dir('git/inttests') {
               deleteDir()
            }
         }
      }
      
// Build the wrapping repository
      stage('wrapping') {
         steps {
            dir('git/wrapping') {
               git credentialsId: 'df028cc4-778d-4f90-ab52-e2a0db283c9f', url: 'git@github.ibm.com:eJATv3/wrapping.git', branch: "${gitBranch}"
         
               sh "mvn --settings ${workspace}/settings.xml -Dmaven.repo.local=${workspace}/repository -P ${mvnProfile} -B -e -fae ${mvnGoal}"
            }
         }
      }
      
// Build the maven repository
      stage('maven') {
         steps {
            dir('git/maven') {
               git credentialsId: 'df028cc4-778d-4f90-ab52-e2a0db283c9f', url: 'git@github.ibm.com:eJATv3/maven.git', branch: "${gitBranch}"
         
               dir('voras-maven-plugin') {
                  sh "mvn --settings ${workspace}/settings.xml -Dmaven.repo.local=${workspace}/repository -P ${mvnProfile} -B -e -fae ${mvnGoal}"
               }
            }
         }
      }
      
// Build the framework repository
      stage('framework') {
         steps {
            dir('git/framework') {
               git credentialsId: 'df028cc4-778d-4f90-ab52-e2a0db283c9f', url: 'git@github.ibm.com:eJATv3/framework.git', branch: "${gitBranch}"
         
               dir('voras-parent') {
                  sh "mvn --settings ${workspace}/settings.xml -Dmaven.repo.local=${workspace}/repository -P ${mvnProfile} -B -e -fae ${mvnGoal}"
               }
            }
         }
      }
      
// Build the core repository
      stage('core') {
         steps {
            dir('git/core') {
               git credentialsId: 'df028cc4-778d-4f90-ab52-e2a0db283c9f', url: 'git@github.ibm.com:eJATv3/core.git', branch: "${gitBranch}"
         
               dir('voras-core-parent') {
                  sh "mvn --settings ${workspace}/settings.xml -Dmaven.repo.local=${workspace}/repository -P ${mvnProfile} -B -e -fae ${mvnGoal}"
               }
            }
         }
      }
      
// Build the common repository
      stage('common') {
         steps {
            dir('git/common') {
               git credentialsId: 'df028cc4-778d-4f90-ab52-e2a0db283c9f', url: 'git@github.ibm.com:eJATv3/common.git', branch: "${gitBranch}"
         
               dir('voras-common-parent') {
                  sh "mvn --settings ${workspace}/settings.xml -Dmaven.repo.local=${workspace}/repository -P ${mvnProfile} -B -e -fae ${mvnGoal}"
               }
            }
         }
      }
      
// Build the runtime repository
      stage('runtime') {
         steps {
            dir('git/runtime') {
               git credentialsId: 'df028cc4-778d-4f90-ab52-e2a0db283c9f', url: 'git@github.ibm.com:eJATv3/runtime.git', branch: "${gitBranch}"
         
               sh "mvn --settings ${workspace}/settings.xml -Dmaven.repo.local=${workspace}/repository -P ${mvnProfile} -B -e -fae ${mvnGoal}"
            }
         }
      }
      
// Build the devtools repository
      stage('devtools') {
         steps {
            dir('git/devtools') {
               git credentialsId: 'df028cc4-778d-4f90-ab52-e2a0db283c9f', url: 'git@github.ibm.com:eJATv3/devtools.git', branch: "${gitBranch}"
         
               sh "mvn --settings ${workspace}/settings.xml -Dmaven.repo.local=${workspace}/repository -P ${mvnProfile} -B -e -fae ${mvnGoal}"
            }
         }
      }
      
// Build the Installation Verification Tests repository
      stage('ivt') {
         steps {
            dir('git/ivt') {
               git credentialsId: 'df028cc4-778d-4f90-ab52-e2a0db283c9f', url: 'git@github.ibm.com:eJATv3/ivt.git', branch: "${gitBranch}"
         
               dir('voras-ivt-parent') {
                  sh "mvn --settings ${workspace}/settings.xml -Dmaven.repo.local=${workspace}/repository -P ${mvnProfile} -B -e -fae ${mvnGoal}"
               }
            }
         }
      }

// Spawn to a docker amd64 agent to build the generic (non-executable) images      
      stage('generic-docker-images') {
         agent { 
            label 'docker-amd64'
         }
	     options {
            skipDefaultCheckout false
         }
         environment {
            def workspace = pwd()
         }
         steps {

// Ensure we force the download of the voras artifacts            
            dir('repository/dev/voras') {
               deleteDir()
            }

            withCredentials([usernamePassword(credentialsId: '633cd4b1-ea8c-4ce1-a6bc-f103009af770', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]){
               sh "docker login -u=${DOCKER_USER} -p=${DOCKER_PASSWORD} ${dockerRepository}"
            }
   
            configFileProvider([configFile(fileId: '86dde059-684b-4300-b595-64e83c2dd217', targetLocation: 'settings.xml')]) {
            }
            
			dir('git/build/docker') {
// Build the maven repository image
			   dir('mavenRepository') {
			      sh "mvn --settings ${workspace}/settings.xml -Dmaven.repo.local=${workspace}/repository -P ${mvnProfile} -B -e clean voras:mavenrepository"
			      
			      sh "docker build -t ${dockerRepository}/voras-maven-repo-generic:$dockerVersion ." 
			      sh "docker push ${dockerRepository}/voras-maven-repo-generic:$dockerVersion" 
			   }
			   
// Build the emedded obr directory
			   dir('dockerObr') {
			      sh "mvn --settings ${workspace}/settings.xml -Dmaven.repo.local=${workspace}/repository -P ${mvnProfile} -B -e clean voras:obrembedded"
			      
			      sh "docker build -t ${dockerRepository}/voras-obr-generic:$dockerVersion ." 
			      sh "docker push ${dockerRepository}/voras-obr-generic:$dockerVersion" 
			   }
			}            
         }
      }
      
// Build all the platform specific docker images
      stage('platform-docker-images') {
         parallel {
            stage('amd64-docker-images') {
               agent { 
                  label 'docker-amd64'
               }
	           options {
                  skipDefaultCheckout false
               }
               environment {
                  def workspace = pwd()
               }
               steps {
            
                  dir('repository/dev/voras') {
                     deleteDir()
                  }

                  withCredentials([usernamePassword(credentialsId: '633cd4b1-ea8c-4ce1-a6bc-f103009af770', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]){
                     sh "docker login -u=${DOCKER_USER} -p=${DOCKER_PASSWORD} ${dockerRepository}"
                  }
   
                  configFileProvider([configFile(fileId: '86dde059-684b-4300-b595-64e83c2dd217', targetLocation: 'settings.xml')]) {
                  }
            
			      dir('git/build/docker') {
			         dir('bootEmbedded') {
			            sh "docker build --pull --build-arg dockerVersion=${dockerVersion} --build-arg dockerRepository=${dockerRepository} -t ${dockerRepository}/voras-boot-embedded-amd64:$dockerVersion ." 
			            sh "docker push ${dockerRepository}/voras-boot-embedded-amd64:$dockerVersion" 
   			         }

			         dir('rasCouchdbInit') {
			            sh "docker build --pull --build-arg dockerVersion=${dockerVersion} --build-arg dockerRepository=${dockerRepository} -t ${dockerRepository}/voras-ras-couchdb-init-amd64:$dockerVersion ." 
			            sh "docker push ${dockerRepository}/voras-ras-couchdb-init-amd64:$dockerVersion" 
   			         }
   			         
			         dir('resources') {
			            sh "docker build --pull --build-arg dockerVersion=${dockerVersion} --build-arg dockerRepository=${dockerRepository} -t ${dockerRepository}/voras-resources-amd64:$dockerVersion ." 
			            sh "docker push ${dockerRepository}/voras-resources-amd64:$dockerVersion" 
   			         }
   			         
			         dir('ibm/bootEmbedded') {
			            sh "docker build --pull --build-arg dockerVersion=${dockerVersion} --build-arg dockerRepository=${dockerRepository} --build-arg platform=amd64 -t ${dockerRepository}/voras-ibm-boot-embedded-amd64:$dockerVersion ." 
			            sh "docker push ${dockerRepository}/voras-ibm-boot-embedded-amd64:$dockerVersion" 
   			         }
			      }            
               }
            }
            stage('s390x-docker-images') {
               agent { 
                  label 'docker-s390x'
               }
	           options {
                  skipDefaultCheckout false
               }
               environment {
                  def workspace = pwd()
               }
               steps {
            
                  dir('repository/dev/voras') {
                     deleteDir()
                  }

                  withCredentials([usernamePassword(credentialsId: '633cd4b1-ea8c-4ce1-a6bc-f103009af770', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]){
                     sh "docker login -u=${DOCKER_USER} -p=${DOCKER_PASSWORD} ${dockerRepository}"
                  }
   
                  configFileProvider([configFile(fileId: '86dde059-684b-4300-b595-64e83c2dd217', targetLocation: 'settings.xml')]) {
                  }
            
			      dir('git/build/docker') {
			         dir('bootEmbedded') {
			            sh "docker build --pull --build-arg dockerVersion=${dockerVersion} --build-arg dockerRepository=${dockerRepository} -t ${dockerRepository}/voras-boot-embedded-s390x:$dockerVersion -f Dockerfile.s390x ." 
			            sh "docker push ${dockerRepository}/voras-boot-embedded-s390x:$dockerVersion" 
   			         }
   			         
			         dir('rasCouchdbInit') {
			            sh "docker build --pull --build-arg dockerVersion=${dockerVersion} --build-arg dockerRepository=${dockerRepository} -t ${dockerRepository}/voras-ras-couchdb-init-s390x:$dockerVersion ." 
			            sh "docker push ${dockerRepository}/voras-ras-couchdb-init-s390x:$dockerVersion" 
   			         }
   			         
			         dir('resources') {
			            sh "docker build --pull --build-arg dockerVersion=${dockerVersion} --build-arg dockerRepository=${dockerRepository} -t ${dockerRepository}/voras-resources-s390x:$dockerVersion ." 
			            sh "docker push ${dockerRepository}/voras-resources-s390x:$dockerVersion" 
   			         }
   			         
			         dir('ibm/bootEmbedded') {
			            sh "docker build --pull --build-arg dockerVersion=${dockerVersion} --build-arg dockerRepository=${dockerRepository} --build-arg platform=s390x -t ${dockerRepository}/voras-ibm-boot-embedded-s390x:$dockerVersion ." 
			            sh "docker push ${dockerRepository}/voras-ibm-boot-embedded-s390x:$dockerVersion" 
   			         }
			      }            
               }
            }
         }
      }

// Build the generic manifests with amd64 and s390x images
      stage('generic-docker-manifests') {
         agent { 
            label 'docker-amd64'
         }
	     options {
            skipDefaultCheckout true
         }
         environment {
            def workspace = pwd()
         }
         steps {
            withCredentials([usernamePassword(credentialsId: '633cd4b1-ea8c-4ce1-a6bc-f103009af770', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]){
               sh "docker login -u=${DOCKER_USER} -p=${DOCKER_PASSWORD} ${dockerRepository}"
            }
            
// boot embedded
			sh "rm -rf ~/.docker/manifests/${dockerRepository}_voras-boot-embedded-$dockerVersion"
			sh "docker pull ${dockerRepository}/voras-boot-embedded-amd64:$dockerVersion"
			sh "docker pull ${dockerRepository}/voras-boot-embedded-s390x:$dockerVersion"
			sh "docker manifest create ${dockerRepository}/voras-boot-embedded:$dockerVersion ${dockerRepository}/voras-boot-embedded-amd64:$dockerVersion ${dockerRepository}/voras-boot-embedded-s390x:$dockerVersion"
			sh "docker manifest push ${dockerRepository}/voras-boot-embedded:$dockerVersion"
            
// the Couchdb RAS initialisation       
			sh "rm -rf ~/.docker/manifests/${dockerRepository}_voras-ras-couchdb-init-$dockerVersion"
			sh "docker pull ${dockerRepository}/voras-ras-couchdb-init-amd64:$dockerVersion"
			sh "docker pull ${dockerRepository}/voras-ras-couchdb-init-s390x:$dockerVersion"
			sh "docker manifest create ${dockerRepository}/voras-ras-couchdb-init:$dockerVersion ${dockerRepository}/voras-ras-couchdb-init-amd64:$dockerVersion ${dockerRepository}/voras-ras-couchdb-init-s390x:$dockerVersion"
			sh "docker manifest push ${dockerRepository}/voras-ras-couchdb-init:$dockerVersion"
            
// The resources image containing maven and the eclipse update site
			sh "rm -rf ~/.docker/manifests/${dockerRepository}_voras-resources-$dockerVersion"
			sh "docker pull ${dockerRepository}/voras-resources-amd64:$dockerVersion"
			sh "docker pull ${dockerRepository}/voras-resources-s390x:$dockerVersion"
			sh "docker manifest create ${dockerRepository}/voras-resources:$dockerVersion ${dockerRepository}/voras-resources-amd64:$dockerVersion ${dockerRepository}/voras-resources-s390x:$dockerVersion"
			sh "docker manifest push ${dockerRepository}/voras-resources:$dockerVersion"
            
// Boot embedded with the IBM CA certificate
			sh "rm -rf ~/.docker/manifests/${dockerRepository}_voras-ibm-boot-embedded-$dockerVersion"
			sh "docker pull ${dockerRepository}/voras-ibm-boot-embedded-amd64:$dockerVersion"
			sh "docker pull ${dockerRepository}/voras-ibm-boot-embedded-s390x:$dockerVersion"
			sh "docker manifest create ${dockerRepository}/voras-ibm-boot-embedded:$dockerVersion ${dockerRepository}/voras-ibm-boot-embedded-amd64:$dockerVersion ${dockerRepository}/voras-ibm-boot-embedded-s390x:$dockerVersion"
			sh "docker manifest push ${dockerRepository}/voras-ibm-boot-embedded:$dockerVersion"
         }
      }
      
// Build the "latest" manifests
      stage('generic-docker-manifests-latest') {
         when {
           environment name: 'GIT_BRANCH', value: 'origin/master'
         }
         agent { 
            label 'docker-amd64'
         }
	     options {
            skipDefaultCheckout true
         }
         environment {
            def workspace = pwd()
         }
         steps {
            withCredentials([usernamePassword(credentialsId: '633cd4b1-ea8c-4ce1-a6bc-f103009af770', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]){
               sh "docker login -u=${DOCKER_USER} -p=${DOCKER_PASSWORD} ${dockerRepository}"
            }
            
			sh "rm -rf ~/.docker/manifests/${dockerRepository}_voras-boot-embedded-latest"
			sh "docker pull ${dockerRepository}/voras-boot-embedded-amd64:$dockerVersion"
			sh "docker pull ${dockerRepository}/voras-boot-embedded-s390x:$dockerVersion"
			sh "docker manifest create ${dockerRepository}/voras-boot-embedded:latest ${dockerRepository}/voras-boot-embedded-amd64:$dockerVersion ${dockerRepository}/voras-boot-embedded-s390x:$dockerVersion"
			sh "docker manifest push ${dockerRepository}/voras-boot-embedded:latest"
			
			sh "rm -rf ~/.docker/manifests/${dockerRepository}_voras-ras-couchdb-init-latest"
			sh "docker pull ${dockerRepository}/voras-ras-couchdb-init-amd64:$dockerVersion"
			sh "docker pull ${dockerRepository}/voras-ras-couchdb-init-s390x:$dockerVersion"
			sh "docker manifest create ${dockerRepository}/voras-ras-couchdb-init:latest ${dockerRepository}/voras-ras-couchdb-init-amd64:$dockerVersion ${dockerRepository}/voras-ras-couchdb-init-s390x:$dockerVersion"
			sh "docker manifest push ${dockerRepository}/voras-ras-couchdb-init:latest"
            
			sh "rm -rf ~/.docker/manifests/${dockerRepository}_voras-resources-latest"
			sh "docker pull ${dockerRepository}/voras-resources-amd64:$dockerVersion"
			sh "docker pull ${dockerRepository}/voras-resources-s390x:$dockerVersion"
			sh "docker manifest create ${dockerRepository}/voras-resources:latest ${dockerRepository}/voras-resources-amd64:$dockerVersion ${dockerRepository}/voras-resources-s390x:$dockerVersion"
			sh "docker manifest push ${dockerRepository}/voras-resources:latest"
            
			sh "rm -rf ~/.docker/manifests/${dockerRepository}_voras-ibm-boot-embedded-latest"
			sh "docker pull ${dockerRepository}/voras-ibm-boot-embedded-amd64:$dockerVersion"
			sh "docker pull ${dockerRepository}/voras-ibm-boot-embedded-s390x:$dockerVersion"
			sh "docker manifest create ${dockerRepository}/voras-ibm-boot-embedded:latest ${dockerRepository}/voras-ibm-boot-embedded-amd64:$dockerVersion ${dockerRepository}/voras-ibm-boot-embedded-s390x:$dockerVersion"
			sh "docker manifest push ${dockerRepository}/voras-ibm-boot-embedded:latest"
         }
      }
   }
}