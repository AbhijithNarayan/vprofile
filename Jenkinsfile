pipeline{
    agent any
  /*   tools{
        maven "MAVEN3"
        jdk "openjdk"
    } */
    environment{
        registry = abhijithnarayan/test    
        registryCredential = 'docker'
        }
    stages{
        /* stage('fetch'){
            steps{
                git branch: 'main ', url:'https://github.com/AbhijithNarayan/vprofile.git'
            }
        } */
        stage('Build'){
            steps{
                sh 'mvn clean install -Dskiptests'
            }
            post {
                success{
                    echo 'now archiving'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Test'){
            steps{
                sh 'mvn test'
            }           
        }
        /* stage('checkstyle analysis'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        } */
        /* stage('CODE ANALYSIS with SONARQUBE') {
          
		  environment {
             scannerHome = tool 'sonar4.7'
          }

          steps {
            withSonarQubeEnv('sonar') {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }

            
          }
        } */
        stage('Build Docker Image') {
            steps(){
                script{
                    dockerimage = docker.build registry + ":V$BUILD_NUMBER"
                }
            }
                
            }
        stage("docker push"){
            steps{
                script{
                    docker.withDockerRegistry('' , registryCredential)
                    dockerimage.push("V$BUILD_NUMBER")
                }
            }
        }
        stage("remove unused image"){
            steps{
                sh 'docker rmi $registry:$BUILD_NUMBER'
            }
        }


    }

}