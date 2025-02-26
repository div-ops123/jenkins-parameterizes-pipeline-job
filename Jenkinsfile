pipeline {
  agent any
  stages {
    stage('Maven Version') {
      steps {
        sh 'echo Print Maven Version'
        sh 'mvn -version'
        // use double-quotes to allow Jenkins pipeline to substitute the values of the parameters into the string before executing
        sh "echo Sleep-Time - ${params.SLEEP_TIME}, Port - ${params.APP_PORT}, Branch - ${params.BRANCH_NAME}"
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests=true'
        archiveArtifacts 'target/hello-demo-*.jar'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
        junit(testResults: 'target/surefire-reports/TEST-*.xml', keepProperties: true, keepTestNames: true)
      }
    }
    
    stage('Local Deployment') {
      steps {
        // sh """ java -jar target/hello-demo-*.jar > /dev/null & """
        sh """ java -jar target/hello-demo-*.jar > app.log 2>&1 & """
      }
    }
    
    stage('Integration Testing') {
      steps {
        sh "sleep ${params.SLEEP_TIME}"
        sh "curl -s http://localhost:${params.APP_PORT}/hello"
      }
    }


  }
  tools {
    maven 'M398'
  }

}
