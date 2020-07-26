node
{
    def mavenHome = tool name: "maven3.6.3"
    
    stage('SourceCode')
    {
    git branch: 'development', credentialsId: '29966239-6a1d-4283-8abe-ab0619405e67', url: 'https://github.com/SureshA3026/maven-web-application.git'
}
stage('BuildArtifact')
{
    sh "${mavenHome}/bin/mvn clean package" 
}
stage('Sonarqube Report')
{
  sh "${mavenHome}/bin/mvn sonar:sonar"   
}
stage('UploadIntoNexus')
{
    sh "${mavenHome}/bin/mvn deploy" 
}
stage('DeployIntoTomcat')
{
 sshagent(['f15a0f07-c36b-4df8-a781-7aafe01e759b'])
{
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@18.223.237.119:/opt/apache-tomcat-9.0.37/webapps"
}
}
stage('EmailNotification')
{
    emailext body: '''Second Build is over.
Regards,
Suresh''', subject: 'Build is over', to: 'capt.sureshreddy@gmail.com'
}
}
