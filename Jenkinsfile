pipeline {
    /* insert Declarative Pipeline here */
    // agent  {  label  'DevOps-Cloud-Node2'  }
    /* agent any */
    /* agent none */
    /* agent { node { label 'RedHat-Cloud-DevOps-Container-1' } } */
    agent {
        node {
            label 'RedHat-Cloud-DevOps-Container-1'
            /* customWorkspace '/home/jenkins/customworkspace' */
        }
    }
    tools {
        // Note: This should match with the tool name configured in your jenkins instance (JENKINS_URL/configureTools/)
        maven "maven"
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5', daysToKeepStr: '5', artifactDaysToKeepStr: '5'))
        /* checkoutToSubdirectory('customworkspace') */
        // disableConcurrentBuilds()
        // disableResume()
        // overrideIndexTriggers(true)
        // preserveStashes(buildCount: 2)
        // quietPeriod(5)
        // retry(2)
        // skipDefaultCheckout(false)
        // skipStagesAfterUnstable()
        // timeout(time: 3, unit: 'MINUTES')
        // timestamps ()
        // parallelsAlwaysFailFast()
    }
    environment {
       MAVEN_HOME = '/opt/maven'
       // This can be nexus3 or nexus2
       // NEXUS_VERSION = "nexus3"
       // This can be http or https
       // NEXUS_PROTOCOL = "http"
       // Where your Nexus is running
       // NEXUS_URL = "rockers873d.mylabserver.com:8081"
       // Repository where we will upload the artifact
       // NEXUS_REPOSITORY = "Jenkins-Repository"
       // Jenkins credential id to authenticate to Nexus OSS
       // NEXUS_CREDENTIAL_ID = "nexus-credentials"
       VERSION = VersionNumber([versionNumberString : '${BUILD_YEAR}.${BUILD_MONTH}.${BUILD_ID}', projectStartDate : '2014-05-19'])
    }
    stages {
       stage ("Checking Java") {
                tools {
                   jdk "java-1.8"
                }
                steps {
                    sh 'java -version'
                }
       }
       stage('Source Clone Section') {
          steps {
              script {
                 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'Git', url: 'https://github.com/praveen252525/First-Pipeline-Project.git']]])
              }
          }
       }
       stage('Cleanup workspace and Version number Sections') {
         steps {
             script {
                // cleanWs()
                sh 'echo "$VERSION"'
             }
         }
       }
       stage("mvn build") {
            steps {
                script {
                    // If you are using Windows then you should use "bat" step
                    // Since unit testing is out of the scope we skip them
                    sh "mvn package -DskipTests=true"
                }
            }
       }
       stage("publish to nexus") {
          steps {
             script {
                // nexusArtifactUploader artifacts: [[artifactId: '${POM_ARTIFACTID}', classifier: '', file: 'target/mv-tom-jfrog-demo.war', type: 'war']], credentialsId: 'nexus', groupId: '${POM_GROUPID}', nexusUrl: 'rockers873c.mylabserver.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'Jenkins-Repository', version: '${POM_VERSION}'
                nexusArtifactUploader artifacts: [[artifactId: 'mv-tom-jfrog-demo', classifier: '', file: 'target/mv-tom-jfrog-demo.war', type: 'war']], credentialsId: 'nexus', groupId: 'mv.tom.jfrog', nexusUrl: 'rockers873d.mylabserver.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'Jenkins-Repository', version: '1.0-SNAPSHOT'
             }
          }
       }
    }
    post  {
        always  {
            echo 'this  can run always'
        }
        success  {
            echo 'this can run only if build is successful'
        }
        failure  {
            echo 'this can run if build gets failure'
        }
        unstable  {
            echo 'this  comes when we see unstable build'
        }
        fixed  {
            echo 'run is successful and the previous run failed or was unstable'
        }
        changed  {
            echo 'this comes when previous build is unsucessful but now the build is successful'
        }
        regression  {
                echo 'run’s status is failure, unstable, or aborted and the previous run was successful'
        }
        aborted  {
                echo 'run has an "aborted" status, usually due to the Pipeline being manually aborted'
        }
        unsuccessful  {
                echo 'stage’s run has not a "success" status'
        }
        cleanup  {
                echo 'after every other post condition has been evaluated, regardless of the Pipeline or stage’s status'
        }
    }
}
