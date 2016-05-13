node {
   stage 'Stage 1'
   echo 'Hello World 1'
   
   stage 'Stage 2'
   echo 'Hello World 2'
   
   stage 'Checkout'
   git 'https://github.com/justincoffman/hello-world-sb.git'
   
   stage 'Build'
   // Pick a container that has maven, but based on Alpine, so that its small
   docker.image('jimschubert/8-jdk-alpine-mvn').inside {
    sh 'mvn --version'
 }
}
