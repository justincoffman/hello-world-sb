node {
   // warning: This whole thing is horribly sub-optimal and dirty
   
   // Pick a container that has maven, but based on Alpine, so that its small. 
   def mvnContainer = docker.image('jimschubert/8-jdk-alpine-mvn')
   
   stage 'Checkout from GitHub'
   git 'https://github.com/justincoffman/hello-world-sb.git'

   stage 'Build & Unit Test'
   mvnContainer.inside {
     sh 'mvn test'
   }

   stage 'Integration Test'
   // Simple (stupid) test against running service
   mvnContainer.inside('-u root:root') {
      sh 'apk add --update curl && rm -rf /var/cache/apk/*'
      
      sh 'mvn install'

      sh 'java -Dserver.port=8090 -jar target/gs-spring-boot-0.1.0.jar &'
      sh 'sleep 10'
      sh '''
          response=$(curl http://localhost:8090);
          if [ "Hello World via Spring Boot" == "${response}" ]; then echo "SUCCESS"; else exit 1; fi;
      '''
   }
   
   stage 'Performance Test'
   // Simple (stupid) test against running service.
   mvnContainer.inside('-u root:root') {
      sh 'apk add --update curl && rm -rf /var/cache/apk/*'
      
      sh 'mvn install'

      sh 'java -Dserver.port=8090 -jar target/gs-spring-boot-0.1.0.jar &'
      sh 'sleep 10'
      sh '''
          responseTime=$(curl -s -w "%{time_total}\n" -o /dev/null http://localhost:8090);
          echo "Reponse time was ${responseTime}"
          if [ 1.0f -gt "${responseTime}" ]; then echo "SUCCESS"; else exit 1; fi;
      '''
   }
   
   stage 'Deploy'
   
}
