pipeline{
    agent any
    stages{
            stage('Build'){
                tools{
                    jdk 'jdk1.8'
                }
                steps
                {
                    echo "Build Project"
                    powershell label: '', script: 'mvn clean package -f spring-boot-samples/spring-boot-sample-atmosphere/pom.xml'
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
                   
                }
            } 
            stage('Publish HTML'){
                steps
                {
                    echo "Publish HTML"
                   
                }
            } 
         }
}   
