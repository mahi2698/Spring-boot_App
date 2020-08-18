pipeline{
    agent any
    stages{
            stage('Build'){
                tools{
                    jdk 'jdk1.8'
                }
                steps
                {
                    script {
                    env.codeanalysis = input message: 'User input required', ok: 'OK!',
                            parameters: [choice(name: 'codeanalysis', choices: ['Yes','No'].join('\n'), description: 'Do you wan to perform code quality analysis?')]
                    }
                    echo "${env.codeanalysis}"

                    when{expression{${env.codeanalysis}=='Yes'}}
                         steps{
                             environment {
                                    SCANNER_HOME = tool 'sonar'
                                  }
                             withSonarQubeEnv('sonar') {
                                        sh '''$SCANNER_HOME/bin/sonar-scanner'''
                             
                                     }

                             echo "Build Project"
                             powershell label: '', script: 'mvn clean package -f spring-boot-samples/spring-boot-sample-atmosphere/pom.xml'
                            }
            }
            }
            stage('Archive'){
                steps
                {
                    echo "Archive Artifact"
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
