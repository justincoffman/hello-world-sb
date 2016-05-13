node {
   
   // Pick a container that has maven, but based on Alpine, so that its small
   def mvnContainer = docker.image('jimschubert/8-jdk-alpine-mvn')
   
   stage 'Checkout from GitHub'
   git 'https://github.com/justincoffman/hello-world-sb.git'
   
   state 'Verify container'
   mvnContainer.inside {
     sh 'mvn --version'
   }
   
   stage 'Build'
   mvnContainer.inside {
     sh 'mvn compile'
   }
   
   stage 'Test'
   mvnContainer.inside {
     sh 'mvn test'
   }
}
