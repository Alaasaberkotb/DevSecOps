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
                  sh "mvn clean verify sonar:sonar \
                      -Dsonar.projectKey=devsecops \
                      -Dsonar.host.url=http://35.172.184.152:30012 \
                      -Dsonar.login=sqp_5d5f0bb38ebda802f9cd2bb9200cb65a3d2c64a4"
                  }
                  timeout(time: 2, unit: 'MINUTES'){
                  script{
                   waitForQualityGate abortPipeline: true
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
      
 //       stage('Docker image build and push'){
  //          steps {
   //             sh "docker build -t hassaneid/java:${BUILD_NUMBER} ."
    //            sh "docker push hassaneid/java:${BUILD_NUMBER}"
      //      }
     //   }
    
   }
}
