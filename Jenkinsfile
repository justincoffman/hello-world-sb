node {
   // warning: This whole thing is horribly sub-optimal and dirty
   
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
   
   def testBranches = [:]
   
   testBranches[0] = {
      stage 'Integration Test'
      node {
         // Simple (stupid) test against running service
         mvnContainer.inside('-u root:root') {
            sh 'apk add --update curl && rm -rf /var/cache/apk/*'
            
            sh 'mvn install'
      
            sh 'java -jar target/gs-spring-boot-0.1.0.jar &'
            sh 'sleep 10'
            sh '''
                #response=$(curl http://localhost:8080);
                #if [ "Hello World via Spring Boot" == "${response}" ]; then echo "SUCCESS"; else exit 1; fi;
            '''
         }
      }
   }
   
   testBranches[1] = {
      stage 'Performance Test'
      node {
         // Simple (stupid) test against running service.
         mvnContainer.inside('-u root:root') {
            sh 'apk add --update curl && rm -rf /var/cache/apk/*'
            
            sh 'mvn install'
      
            sh 'java -jar target/gs-spring-boot-0.1.0.jar &'
            sh 'sleep 10'
            sh '''
                responseTime=$(curl -s -w "%{time_total}\n" -o /dev/null http://localhost:8080);
                #echo "Reponse time was ${responseTime}"
                #if [ 1.0 -gt "${responseTime}" ]; then echo "SUCCESS"; else exit 1; fi;
            '''
         }
      }
   }
   
   parallel testBranches
   
   
   
   stage 'Deploy'
   
}
