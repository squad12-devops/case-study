pipeline {
  agent any
  stages {
    stage('git') {
      steps {
        echo 'Cloning Git hub jenkins file'
        git(credentialsId: 'Github', url: 'https://github.com/squad12-devops/DevOps-Demo-WebApp.git')
      }
    }

    stage('Static Code Analysis') {
      steps {
        echo 'validating project'
        sh 'mvn validate'
      }
    }
    stage('Sonarqube Analysis') {
           steps{
               withSonarQubeEnv(credentialsId: 'sonar', installationName: 'sonarqube') {
                 sh "${tool("sonarqube")}/bin/sonar-scanner \
                -Dsonar.projectKey=. \
                -Dsonar.sources=. \
                -Dsonar.tests=. \
                -Dsonar.inclusions=**/test/java/servlet/createpage_junit.java \
                -Dsonar.test.exclusions=**/test/java/servlet/createpage_junit.java \
                -Dsonar.login=admin \
                -Dsonar.password=sonar "
                 sh 'mvn validate -f pom.xml'
                }
           }
       }

    stage('Compile') {
      steps {
        echo 'compiling project'
        sh 'mvn compile'
      }
    }

    stage('Build') {
      steps {
        echo 'build package'
        sh 'mvn package'
      }
    }

    stage('Last Step') {
      steps {
        echo 'Test step slack'
        slackSend channel: '#squad12', message: 'Build successful'
        rtUpload (
              serverId: 'devopsfrog',
              specPath: '/var/jenkins_home/workspace/TestJenkinsPipeline11/target/AVNCommunication-1.0.war',
    
              buildName: 'holyFrog',
              buildNumber: '42'
          )
      }
    }

  }
  environment {
    PATH = "/var/jenkins_home/apache-maven-3.5.4/bin:$PATH"
  }
}
