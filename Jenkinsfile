node{

def mavenHome = tool name: 'maven3.9.2'
echo "Job name is : $JOB_NAME"
echo "Node name is : $NODE_NAME"
echo "Jenkins Home is $JENKINS_HOME"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckOutCode'){
git branch: 'development', credentialsId: '66a19b15-c5e1-476c-a937-fe75507f9659', url: 'https://github.com/ManasaSP/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport') {
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcatServer'){
sshagent(['2d2d8684-73d3-489b-9121-6ff1b59d238b']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.41.110:/opt/apache-tomcat-9.0.75/webapps"
}
}
}
