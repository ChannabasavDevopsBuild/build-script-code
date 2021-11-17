node {
    
    //echo "GitHub BranhName ${env.BRANCH_NAME}"
  //echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
  
  properties([
    buildDiscarder(logRotator(numToKeepStr: '3')),
    pipelineTriggers([
        pollSCM('* * * * *')
    ])
  ])
    
    
    def mavenHome = tool name: "maven3.8.3"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '10', daysToKeepStr: '', numToKeepStr: '10')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('*  *  *  *  *')])])
    
    stage('CheckOutCode'){
    git branch: 'development', credentialsId: '9b046676-1f23-44d2-b726-190784d60089', url: 'https://github.com/channabasav-1991/maven-web-application.git' 
    }
    stage('Build'){
    sh "${mavenHome}/bin/mvn clean package"
    }
    stage('ExecuteSonarQubeReport'){
    sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('UploadArtifactsIntoNexusRepo'){
    sh "${mavenHome}/bin/mvn deploy"
    }
    stage ('DeployApplicationIntoTomcatServer'){
    sshagent(['6b8149b3-e0c3-43a4-8d7e-48ddfeee0c5b']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.127.32.173:/opt/apache-tomcat-9.0.54/webapps/"
    }
    }
    /* stage('SendEmailNotification'){
    mail bcc: '', body: '''Bulid is over ..

    Regards
    Channabasav''', cc: 'pankupatil@gmail.com', from: '', replyTo: '', subject: 'Bulid is over ', to: 'pankupatil@gmail.com'    
    }
    */
}
