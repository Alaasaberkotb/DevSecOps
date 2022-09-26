pipeline {
  agent any
    stages {
        stage('Unit Tests - JUnit and Jacoco') {
            steps {
                  sh "mvn test"
            }
        }
        stage('Mutation Tests PIT') {
            steps{ 
              sh "mvn org.pitest:pitest-maven:mutationCoverage" 
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
      //  stage('SonarQube - SAST') {
       //     steps {
        //          withSonarQubeEnv('SonerQube'){
        //          sh "mvn sonar:sonar -Dsonar.projectKey="----" -Dsonar.host.url=http://devsecops-demo.eastus.cloudapp.azure.com:9000"
    //}
             //    timeout(time: 2, unit: 'MINUTES'){
        //      script{
        //            waitForQualityGate abortPipeline: true
    //}
    //         }
         //   }
       // }
<<<<<<< HEAD
    //    stage('Vulnerability Scan Docker'){
     //       steps{
      //          sh "mvn dependency-check:check"
       //     }
       // }
=======
   //     stage('Vulnerability Scan Docker'){
   //         steps{
       //         sh "mvn dependency-check:check"
         //   }
   //     }
>>>>>>> 265596d0ba3b2465e9ee1ba25bdb5d7e7253158c
        stage('Docker image build and push'){
            steps {
                sh "docker build -t hassaneid/java:${BUILD_NUMBER} ."
                sh "docker push hassaneid/java:${BUILD_NUMBER}"
            }
        }
    
   }
    post{
        always{
            junit 'target/surefireReports/*.xml'
            jacoco execPattern: 'target/jacoco.exec'
            pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
<<<<<<< HEAD
    //        dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
=======
        //    dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
>>>>>>> 265596d0ba3b2465e9ee1ba25bdb5d7e7253158c
        }
    }
}
