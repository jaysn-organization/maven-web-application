node {
    def mavenHome = tool name : 'maven3.9.5'
    echo "the Job name is : ${env.JOB_NAME}"
    echo "Jenkins Home Dir is : ${env.JENKINS_HOME}"
    echo "The Jenkins node name is :${env.NODE_NAME}"
    echo "The Build number is : ${env.BUILD_NUMBER}"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], pipelineTriggers([pollSCM('* * * * *')])])
    stage ('CheckoutCode'){
       git branch: 'development', credentialsId: 'c0fb0619-7292-4e0b-a629-42582dca4d4a', url: 'https://github.com/jaysn-organization/maven-web-application.git'
    }
    stage ('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage ('ExecuteSonarQubeReport'){
        sh "${mavenHome}/bin/mvn clean sonar:sonar"
    }
    stage ('UploadArtifactIntoNexus'){
        sh "${mavenHome}/bin/mvn clean deploy"
    }
    stage ('DeployAppIntoTomcat'){
        sshagent(['7c6dec6f-1730-49bd-b6e6-fc280f8ba2b6']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.0.74.252:/opt/apache-tomcat-9.0.94/webapps/"
}
    }
}
