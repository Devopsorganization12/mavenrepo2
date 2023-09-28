pipeline{ 
  agent any
  
tools{
maven 'maven3.9.4'
}

triggers {
  pollSCM '* * * * *'
}

stages{
    
stage('git'){
steps{
checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Devopsorganization12/mavenrepo2.git']])
}
}

stage('build'){
steps{
sh 'mvn package'
}
}

stage('sonar'){
steps{
    
withSonarQubeEnv('sonarqube') {
sh 'mvn sonar:sonar'

}
}
}
stage('nexus'){
steps{
nexusArtifactUploader artifacts: [[artifactId: 'maven-compiler-plugin', classifier: '', file: 'target/studentapp-2.5-SNAPSHOT.war', type: 'war']], credentialsId: 'nexus', groupId: 'com.jdevs', nexusUrl: '65.0.104.53:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '2.5-SNAPSHOT'
}
}
stage('tomcat'){
steps{
deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://15.207.19.76:8080')], contextPath: null, war: '**/*.war'
}
}
stage('email'){
steps{
emailext body: '$DEFAULT_CONTENT', postsendScript: '$DEFAULT_PRESEND_SCRIPT', presendScript: '$DEFAULT_PRESEND_SCRIPT', replyTo: '$DEFAULT_REPLYTO', subject: '$DEFAULT_SUBJECT', to: 'vegesna.sarada.555@gmail.com'
}
}
}
  
}
