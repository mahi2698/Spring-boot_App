pipeline{
    agent any
       stages{
            stage('Build'){
                            tools{
                                jdk 'jdk1.8'
                                 }
                            def userInput = input message: 'Do you want to perform code quality analysis?', parameters: [choice(choices: ['Yes', 'No'], description: '', name: 'Choices')]
                            when { ${userInput} == 'Yes' }
                                    steps {
                                            withSonarQubeEnv() { // If you have configured more than one global server connection, you can specify its name
                                            sh "/bin/sonar-scanner"
                                            echo "Build Project"
                                            powershell label: '', script: 'mvn clean package -f spring-boot-samples/spring-boot-sample-atmosphere/pom.xml'
                                          }
                            
                             when { ${userInput} == 'No' }
                                    steps {
                                            echo "Build Project"
                                            powershell label: '', script: 'mvn package sonar:sonar -f spring-boot-samples/spring-boot-sample-atmosphere/pom.xml'
                                          }
                            }
                
            stage('Archive'){
                steps
                {
                    echo "Archiving Artifact"
                    archiveArtifacts artifacts: 'spring-boot-samples/spring-boot-sample-atmosphere/target/*.jar', followSymlinks: false
                }
            } 
            stage('Publish JUnit'){
                steps
                {
                    echo "Publish JUnit"
                    junit 'spring-boot-samples/spring-boot-sample-atmosphere/target/surefire-reports/*.xml'                   
                }
            } 
            stage('Publish HTML'){
                steps
                {
                    echo "Publish HTML"
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: 'spring-boot-samples/spring-boot-sample-atmosphere/target/site/jacoco', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
                }
            } 
         }
}   
