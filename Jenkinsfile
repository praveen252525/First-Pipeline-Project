pipeline {
    /* insert Declarative Pipeline here */
    agent  {  label  'DevOps-Cloud-Node2'  }
}
options {
        buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5', daysToKeepStr: '5', artifactDaysToKeepStr: '5'))
}
stages {
    stage(Source Clone Section) {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'Git', url: 'https://github.com/praveen252525/First-Pipeline-Project.git']]])
    }
}
