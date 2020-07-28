node
{
def mavenHome = tool name: "maven-3.6.3"

properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])

stage('checkoutcode')
{
git branch: 'development', credentialsId: '5975a322-aa26-48dc-a271-2ff07428742d', url: 'https://github.com/CS-EC-Projects/Snapdeal.git'
}
stage('build')
{
sh "${mavenHome}/bin/mvn clean package"
}
stage('executeSonarQube Report')
{
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('uploadArtificatsinto nexus')
{
sh "${mavenHome}/bin/mvn clean deploy"
}
stage('deploy into tomcat')
{
sshagent(['tomcatuser']){
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@18.220.161.15:/opt/apache-tomcat-9.0.37/webapps"
}
}
stage('send email notification')
{
emailext attachLog: true, body: '''build is over.. successfully deployed.

Regards,
C.Chandra shekar,
8712312237
shekar.srit@gmail.com''', subject: 'build is over', to: 'shekar.srit@gmail.com'
}
}
