node {
  
  stage('Clone') {
      dir('.') {
          git branch: 'main', credentialsId: 'github_com', url: 'git@github.com:Sowmya1063/example-java-maven-jenkins-scripted.git'
      }    
  }       

  stage('Deploy') {
     withCredentials([usernamePassword(
        credentialsId: 'github_maven', 
        passwordVariable: 'MVN_PASSWORD', 
        usernameVariable: 'MVN_USERNAME')]) {

        withMaven(mavenSettingsFilePath: 'settings.xml') {
          sh """
            ./mvnw deploy \
                -Drepo.id=github \
                -Drepo.login=Sowmya1063 \
                -Drepo.pwd=Akk@2003\
                -Drevision=1.${BUILD_NUMBER}
          """
        }  
     }
  }  

  stage('Post') {
    jacoco()
    junit 'target/surefire-reports/*.xml'
    def pmd = scanForIssues tool: [$class: 'Pmd'], pattern: 'target/pmd.xml'
    publishIssues issues: [pmd]
  }
  
}
