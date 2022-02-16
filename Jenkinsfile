node{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], 
                
//    pipelineTriggers([pollSCM('* * * * *')])])

    def mavenHome = tool name: "maven3.8.3"
//Get code from Github repo
stage('CheckoutCode'){
    git branch: 'development', credentialsId: '47a895a3-af49-4c95-bd67-6081726da2ff', url: 'https://github.com/devops-vali2021/maven-web-application.git'
}  
//Using maven to perform build
stage('Build'){
    sh "${mavenHome}/bin/mvn clean package"
}
//to execute sonarqube report
stage('ExecuteSonarQubeReport'){
    sh "${mavenHome}/bin/mvn sonar:sonar"
}
//To upload artifact into Nexus Repo
stage('UploadArtifactIntoNexusRepo'){
    sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppToTomcatServer'){
    sshagent(['949cb00c-8170-4324-8a4c-cfe343240a55']) {
    // some block
    
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.28.96:/opt/apache-tomcat-9.0.56/webapps"
}
}
}//Node Close
