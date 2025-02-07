pipeline {

  agent any

  tools{
    maven 'mvn3.8.6'
  }

    stages {
        
        stage('Unit Tests - JUnit and Jacoco') {
            steps {
                  sh "mvn test"
                  junit 'target/surefire-reports/*.xml'
                  jacoco execPattern: 'target/jacoco.exec'
            }
        }
      
        stage('Mutation Tests PIT') {
            steps{ 
              sh "mvn org.pitest:pitest-maven:mutationCoverage" 
              pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
                 } 
        }
        
        
        stage('SonarQube - SAST') {
            steps {
              withSonarQubeEnv('sonar'){
                  sh "mvn clean verify sonar:sonar \
                      -Dsonar.projectKey=dev \
                      -Dsonar.host.url=http://3.91.70.141:9000"
                        }
                  }
            
        }
      
        stage('Vulnerability Scan Dependencies'){
            steps{
                sh "mvn dependency-check:check"
                dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
            }
        }
      
        stage('Build JAR') {
            steps {
                  sh "mvn clean install -DskipTests=true"
            }
        }
      
        stage('Artifacts JAR') {
            steps {
                 archiveArtifacts artifacts: 'target/*.jar'
            }
        }
     
   }
}
