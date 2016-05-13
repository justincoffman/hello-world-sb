node {
   
   // Pick a container that has maven, but based on Alpine, so that its small
   def mvnContainer = docker.image('jimschubert/8-jdk-alpine-mvn')
   
   stage 'Checkout from GitHub'
   git 'https://github.com/justincoffman/hello-world-sb.git'
   
   stage 'Verify container'
   mvnContainer.inside {
     sh 'mvn --version'
   }
   
   stage 'Build & Unit Test'
   mvnContainer.inside {
     sh 'mvn test'
   }
   
   stage 'Integration Test'
   mvnContainer.inside {
     sh 'mvn install'
     
     // TODO: separate mvn install from java run/test
     sh '''
            apk --update add java curl
          '''
          
     sh 'curl http://www.google.com'
     sh 'java --version'
   }
}
